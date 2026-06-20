# Action flows (server-side workflows)

## Actionflow Domain Knowledge
Actionflows are server-side workflows for business logic. They run entirely on the server;
the frontend triggers them via "Request – Actionflow" component actions.

### Execution Modes
Synchronous: caller blocks for the result; ACID (any node error rolls back all DB writes).
Asynchronous: fire-and-forget; only the failing node's DB writes roll back; required for
  AI agents and video generation.

Both modes share a per-flow total timeout (`ACTION_FLOW_TIMEOUT_MILLISECONDS`): 15 s on
free-tier servers, 10 min on paid tiers. The budget is for the entire flow, not per node.

### Triggers
Manual (UI action), scheduled (cron), database-change event, webhook.

### Node Types
Database: query/insert/update/delete on a table.
Call API: invoke a configured API; bind response to output params.
Run AI: execute a ZAI agent (async only).
Run Actionflow: call another flow (sync cannot call async).
Set Variable: assign values to declared flow-level variables.
Current User: get current user's ID / Access Token.
Permissions: grant or revoke roles for a user.
Run Code: execute custom JavaScript.
Files: fetch/convert external images/videos/files to Momen native types.
Condition: branch logic evaluated left to right.
Loop: iterate over a list; inner nodes access item data.
Send SMS - Twilio: send SMS messages.
Gemini Veo 3.1: generate 8-second video clips (async only).

### Variables
Declared at flow level, accessible by all nodes, assigned with "Set Variable" node.

### Error Handling
Synchronous: all DB changes roll back on error.
Asynchronous: only the failing node's DB changes roll back.

### Versioning
Each save creates a new version. "Sync Backend" required after editing for changes to
take effect in production.

### JavaScript Sandbox (Run Code node)
Custom Code Blocks run in a synchronous GraalJS server sandbox. No async/await,
no require(), no browser APIs. All platform interactions go through the global
`context` object:
  context.getArg(key)                              — retrieve flow input
  context.setReturn(key, val)                      — pass output to downstream nodes
  context.runGql(query, variables, options)        — execute DB operations
  context.callThirdPartyApi(apiId, params)         — invoke a configured REST API
  context.callActionFlow(flowId, ver, args)        — run a sub-flow synchronously
  context.createActionFlowTask(flowId, ver, args)  — trigger a sub-flow async
  context.getSsoUserInfo()                         — get authenticated user profile
  context.uploadMedia(url, headers)                — stream remote image to asset server
  context.log(message)                             — emit to Log Service

## How to drive it (CLI only)

All commands are `momen-mcp <verb>`. A long-lived daemon holds the in-memory CRDT schema session
between calls. **Edits do NOT go live until `schema save` + `project sync-backend`.**

```bash
momen-mcp whoami                                    # check auth; if needed: momen-mcp login
momen-mcp project set-current --projectExId <exId>  # pin the project (momen-mcp projects search to find it)
momen-mcp schema load                               # warm the schema session
```

Operations run through one verb:

```bash
momen-mcp schema tool-call --toolCalls '[{"name":"<TOOL_NAME>","args":{ ... }}]' [--apply]
```
Omit `--apply` for a dry run; add it to upload the CRDT patch. Batch several calls in one array.

## Operation reference (`schema tool-call` names)

| Intent | `name` | Required `args` |
|---|---|---|
| List flows | `GET_ALL_ACTION_FLOW_INFOS` | — |
| Flow detail (ids) | `GET_ACTION_FLOW_DETAIL` | `actionFlowId` |
| Add flows | `ADD_ACTION_FLOWS` | `items` |
| Add a node | `ADD_ACTION_FLOW_NODE` | `actionFlowId`, `afterNodeId`, `node` |
| Add output fields | `ADD_ACTION_FLOW_OUTPUT_FIELDS` | `actionFlowId`, `items` |

> Output on **pre-refactor** projects uses `ADD_ACTION_FLOW_OUTPUT_FIELDS` / `DELETE_ACTION_FLOW_OUTPUT_FIELDS`. On post-refactor projects use `SET_ACTION_FLOW_OUTPUT` instead.

AI / video nodes must be async (`isAsync=true`). Discover node/ids via `GET_ACTION_FLOW_DETAIL`; fill node value bindings with `data-binding.md`.

Then ship:

```bash
momen-mcp schema validate && momen-mcp schema save && momen-mcp project sync-backend
```
