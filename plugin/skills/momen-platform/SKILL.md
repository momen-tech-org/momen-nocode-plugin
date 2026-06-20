---
name: momen-platform
description: >-
  Orientation and entry point for working on a Momen project. Load first for any Momen app task — designing or changing the data model, UI, server-side action flows, permissions, payments, or data bindings, or reading runtime logs — then follow it to the right capability. For Momen projects only; not generic programming help.
---

# Momen platform orientation

Momen is a powerful no-code development platform that enables developers to build
and deploy fully functional web and mobile applications. It integrates a visual drag-and-drop design canvas, an
enterprise-grade relational database engine (PostgreSQL), and an event-driven backend logic
execution pipeline.

This overview details the structural design, data engines, execution pipelines,
and safety requirements of the platform.

---

## 1. Architectural Philosophy (The Three Pillars)

Momen decouples application systems into three isolated yet bound layers:

1. **Design**: The presentation layer, built strictly on nested HTML components obeying
   the CSS Box Model and CSS Flexbox layouts.
2. **Data**: The persistence layer, powered by a built-in, highly-scalable relational
   database engine (PostgreSQL). All UI contents are dynamically populated by binding
   to database rows or API schemas.
3. **Action**: The behavior layer, using an event-driven "Trigger -> Execution" model
   that connects frontend components to local client tasks or backend operations.

---

## 2. The Mental Model & Engineering Workflow

Developing with Momen requires a rigorous **data-first** engineering approach. LLMs must
outline solutions in this exact order:

1. **Data Modeling**: Design the Entity-Relationship (ER) diagram, database tables,
   constraints, and relationships first.
2. **UI Layout**: Construct nested structural containers (Views) and map static
   display/input components onto them.
3. **Logic & Binding**: Link dynamic variables/fields to inputs/displays, then map
   actions or backend workflows to events.
4. **Verification**: Run diagnostic reviews using edit-time error panels and runtime
   log tracing before deploying.

---

## 3. Database Architecture & Data Binding

Momen runs on an enterprise-grade relational database powered by PostgreSQL.

### 3.1 Relationship Modeling
All table relations map directly to standard SQL constraints:
* **One-to-One (1:1)**: e.g., `account` <-> `user_profile`. Direct foreign key binding.
* **One-to-Many (1:N)**: e.g., `account` -> `post` (One author, multiple posts).
* **Many-to-Many (N:M)**: Must be configured using a **junction/intermediate table**
  with two distinct 1:N relations. e.g., `account` -> `enrollment` <- `course`
  represents a Many-to-Many relation between students and courses.

### 3.2 State and Variable Scopes
Momen distinguishes scopes by lifecycle and visibility:

**Application scope** (lifetime = app session):
* **Global Variable**: shared across all pages. Theme, feature flags, cached user state.
* **Current User**: the logged-in user object — id, profile fields, SSO claims.

**Page scope** (lifetime = current page):
* **Page Parameter**: passed from the caller. Path params (`/product/:id`) and query
  params (`?q=abc`) are both Page Parameters.
* **Page Variable**: local UI/form state — active tab, search keywords, loading flag.
* **Page Data**: result of the page's Page Data Source (a remote query the page binds at load).
* **Component State**: the value of stateful input components (Input, Select, Switch).
  Two-way binding writes flow back into Component State automatically.

**List-iteration scope** (lifetime = single rendered row):
* **Current Item**: the row object inside a List component. Child components inside the
  list bind to `Current Item.field` instead of re-querying the table.

**Action-Flow scope** (lifetime = single flow execution):
* **Action Input**: parameters declared on the flow's Start node.
* **Flow Variable**: locals declared by a "Set Variable" node; visible to subsequent nodes
  in the same flow.
* **Flow Output**: the payload returned by the "Return" node — visible to the caller.
* **Loop Item**: the current iteration value inside a Loop node.

### 3.3 Data Interaction Types
Momen splits operations into two non-overlapping categories to prevent infinite render loops:
* **Query (Read-Only)**: Idempotent operations that fetch data. Used directly as dynamic data
  sources for Lists, Select Views, and Pages.
* **Mutation (State-Changing)**: Operations that insert, update, or delete records. These
  **must** be explicitly triggered as actions (e.g., On Click events) and can never be used
  as static data sources.

### 3.4 Data Binding
Data binding connects data sources to component properties. A property can be set to:
* **OPTION**: a live value from the hierarchical data binding options tree — sources include
  database table fields, page variables, global variables, logged-in user, list item context
  ("Current Item"), and actionflow inputs/outputs. The exact `pathInHierarchicalMenu` must
  match the options tree; never fabricate a path.
* **CONST_VALUE**: a literal constant (string, number, boolean).
* **DISPLAY_NAME**: rename the component's displayed label.

Lists render one row per record from their data source. Children inside a list bind to the
list's "Current Item" context (a scoped reference to that row's fields) rather than querying
a new table — this preserves the relational link.

---

## 4. Visual Layout & Positioning Engine

Momen's canvas operates as a visual CSS rendering engine.

### 4.1 Positioning Methods
* **Relative (Default)**: Components follow the natural page layout flow, stacking rows/columns
  according to parent Flexbox rules. Moving a sibling pushes surrounding elements.
* **Absolute**: Removed from the natural document flow. Positioned relative to the boundaries
  of its **immediate parent** container, ignoring other siblings.
* **Fixed**: Locked to the viewport (browser window). Stays static even when scrolling.

### 4.2 Views & Conditional Views
* **Container (View)**: Wraps child elements, serving as a Flexbox box (Horizontal/Vertical arrangement).
* **Conditional View**: Act as a logical switch statement. Contains multiple distinct child
  canvases (Cases). At runtime, only the single canvas corresponding to the true evaluation
  condition is rendered in the DOM, keeping the page clean and secure.

---

## 5. Logic Engines: Frontend vs. Backend

### 5.1 Frontend Actions
Lightweight operations executing directly in the client browser/app. Triggered by UI events.

**Direct database mutations**: The frontend can insert/update/delete rows directly, without
routing through an Actionflow, **when both conditions hold**:
1. No ACID requirement (a partial failure across operations is acceptable), and
2. No trust concern (the user has no incentive to forge or tamper with the request — e.g.,
   liking your own post, marking your own task done, incrementing a personal counter).
When either condition fails (cross-table consistency needed, or a malicious client could
exploit the operation), the mutation must go through a Backend Actionflow instead — see §6.

**Other typical frontend actions**:
* Navigation: push page, redirect, back, new tab, external link.
* Feedback: show/hide Toast, Modal, loading overlay.
* Local state: set Page Variable or Global Variable.
* Data refresh: Refresh (reload a remote data source), List Control (scroll-to / load-more).
* File operations: upload (opens picker), download, clipboard copy, QR scan.
* AI agent invocation: "Run AI" triggers a ZAI agent — returns streamed text or structured
  JSON; multi-turn via sessionId.
* Trigger Actionflow: "Request – Actionflow" calls a backend flow with input params and
  receives its outputs; use this for CRUDs that require ACID or trust.
* Auth: Login, Logout, SSO Login.
* Payment: Stripe.

### 5.2 Backend Actionflows
Server-side visual execution graphs used to perform robust operations. They are triggered by:
* Client requests (explicit frontend action calls).
* Cron schedules (scheduled background workflows).
* Webhooks (third-party external integrations).
* Database Triggers (automatically runs on `INSERT`/`UPDATE`/`DELETE` of target tables).

**Node types available inside an Actionflow:**
Database (query/insert/update/delete on a table), Call API, Run AI (async only),
Run Actionflow (sub-flow; sync cannot call async), Set Variable, Current User,
Permissions (grant/revoke roles), Run Code (JavaScript), Files (fetch/convert external
media), Condition (branching), Loop (iterate a list), Send SMS via Twilio, Gemini Veo 3.1
video generation (async only).

**Execution modes**: Synchronous flows are ACID — any node failure rolls back all DB
changes and the caller blocks waiting for the result. Asynchronous flows are fire-and-
forget; only the failing node's DB changes roll back; required for AI agents and video
generation. Both modes share a **per-flow total timeout** governed by
`ACTION_FLOW_TIMEOUT_MILLISECONDS` — 15 s on free-tier servers, 10 min (600 s) on paid
tiers. The budget is for the whole flow, not per node.

---

## 6. Architectural Security Principles

LLMs must enforce the **Zero Trust Client** model when designing Momen architectures:

1. **Untrusted Client Rule**: The client browser is entirely compromiseable. Never calculate
   prices, validate stock counts, or verify user authorization clearance on the frontend.
2. **Secure Transaction Pattern**:
   * Frontend calls a Backend Actionflow, passing only the `product_id`.
   * The Actionflow queries the product table *on the server* to fetch the immutable price.
   * The Actionflow inserts a secure `Pending` order in the database and returns the
     generated `order_id` and the verified price.
   * Frontend maps the *returned* `order_id` and verified price to the payment action.
3. **Webhook Verification**: Business state mutations must **only** occur inside the backend
   Webhook actionflow triggered directly and securely by the Payment Gateway, never from a
   frontend callback.

### 6.4 Data Isolation & Access Boundaries (Relation-First Hybrid Pattern)
Whenever a system requires segmented data access (e.g., multi-tenant, department, region, client, or creator separation):
1. **Avoid Role Explosion:** Do NOT create separate roles for every individual organizational entity or group (e.g., do not make separate roles for every department or store branch).
2. **The Hybrid ABAC Pattern (Relation-First):**
   - Instead of manually creating a `group_id` or `tenant_id` column (which is strictly forbidden by database conventions), **create a one-to-many (1:n) RELATION** from your context table (e.g., `department` or `tenant`) to the `Account` table. This auto-generates a non-editable foreign key column `department_id` or `tenant_id` on the `Account` table.
   - Similarly, **create a one-to-many (1:n) RELATION** from the same context table to your business/data table (e.g., `sales_record`), which auto-generates the corresponding FK column on the business table.
   - Define a single generic role representing the user's functional capability (e.g., `Staff`).
   - Configure Row-Level Security (RLS) filters inside that role's table permissions to dynamically isolate rows at query time by comparing the auto-generated FK columns:
     Table.group_id == Account.group_id

---

## 7. Environment Lifecycle Model

* **Mirror**: Hot-reloading, local sandbox. Instantly reflects UI edits in a local window.
  No build compile or backend synchronization is required during rapid frontend iteration.
* **Preview**: Compiles and builds the current source into a staging release, providing a
  unique staging URL. Necessary for deep verification testing of custom components and
  payment simulations.
* **Publish**: Deploys the code to the production environment for live users. If database
  tables, APIs, or Actionflows were modified, they must be synchronized using **Sync Backend**
  to apply schema migrations to production servers.

---

## 8. Diagnostics and Troubleshooting Framework

1. **Check Edit-time Warnings (Error Collector)**:
   * Look for unmapped input parameters, invalid expressions, or variable mismatch warnings
     listed under Momen's navigation bar error console.
2. **Retrieve execution details (Log Service)**:
   * Query the runtime logs using the exact `traceId` attached to the failed request.
   * Trace backend code block execution steps, comparing timestamp drift and evaluating
     input parameters.
3. **Database Constraints Validation**:
   * If database actions throw `unique_constraint_violation` during data imports or
     integrations, check if the conflict resolution rule is set to `Skip` or `Update`
     on the unique fields.

---

## 9. Platform-Specific Features

### Deployment Targets
* **Web**: Standard browser application with a unique URL.
* **Mobile**: Native iOS/Android app.
WeChat Mini Program is not available on Momen.

### Payment Gateways
* **Stripe**: Credit/debit card and other Stripe-supported payment methods.

### Authentication Providers
* Google (OAuth)
* Facebook (OAuth)
* Email/password

### Timezone & Locale
Default timezone: **UTC+00:00**.
Default UI language: English (en-US).

## Capabilities

Each capability is a sibling file in this skill folder. When a task calls for one, read that file for its domain rules and `momen-mcp` CLI recipes. Work data-first: build the data model before the UI that displays it and the action flows that mutate it.

- **Database schema (tables, fields, relations, constraints)** (`schema-table.md`) — Create, modify, or delete database tables, columns, relations, and unique constraints. Use when designing or changing the data model (adding a table, field, relation, or constraint), or when you need column-type rules, naming conventions, or system-table constraints.
- **Data binding** (`data-binding.md`) — Connect a UI component to the data it displays: populate a list or text from a database query, a page or global variable, or the logged-in user; set a literal constant; or rename a component's title. Use when an existing component needs live data or a value bound to it, rather than to create or lay out the component itself.
- **UI components & layout** (`ui-component.md`) — Inspect and edit the UI component tree (pages, containers, and leaf components). Use when adding, updating, or deleting components, or when you need component types, slot requirements for LIST/TAB_VIEW/SELECT_VIEW, Flexbox layout, or responsive breakpoints.
- **Action flows (server-side workflows)** (`actionflow.md`) — Inspect and edit server-side action flows: the workflows that run business logic on a button press, a schedule (cron), a database-change event, or a webhook. Use when adding or changing backend logic such as querying or writing the database, calling an API, running custom JavaScript, branching or looping, or choosing synchronous (transactional) vs. asynchronous execution.
- **Runtime logs & debugging** (`runtime-logs.md`) — Fetch and interpret server-side runtime logs. Use when debugging a runtime error, when the user shares a traceId, or when they ask about server logs (covers log types like ACTION_FLOW, GATEWAY, SQL_GENERATION and Elasticsearch query syntax).
- **Permissions (RBAC + ABAC)** (`permissions.md`) — Design RBAC + ABAC access control and guide its setup. Use when the project involves multiple user groups, data isolation, restricted actions, 403 errors, or table/column/row permissions and relation-first tenant isolation.
- **Payments (Stripe)** (`payments.md`) — Design and wire up payments and guide their setup. Use when the project involves checkout, orders, subscriptions, or refunds (covers order-table design, the auto-created payment Actionflows, the webhook idempotency contract, and secure order creation).

## Driving a project from the CLI

```bash
momen-mcp login                                     # browser auth (once)
momen-mcp projects search --projectName "My App"    # find the project exId
momen-mcp project set-current --projectExId <exId>  # pin it
momen-mcp project metadata                          # plan, capabilities, deployment
momen-mcp schema load                               # warm the data-model session
#   ... read the matching capability sub-document (see Capabilities above), then edit ...
momen-mcp schema validate && momen-mcp schema save && momen-mcp project sync-backend
```
