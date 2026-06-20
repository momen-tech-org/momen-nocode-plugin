# Data binding

## Data Binding Domain Knowledge
Data binding connects data sources to UI components for display or interaction.

### Concepts
Schema path: array of keys/indices locating a bindable node in the project JSON schema.
Data binding options tree: hierarchical tree of all available data for a given schema path.

### Binding Kinds
OPTION: bind to an existing path in the data binding options tree.
  pathInHierarchicalMenu must be the EXACT complete path. Never guess or fabricate.
  If the tree root is a named context (e.g., "List Context"), include it as the first element.
CONST_VALUE: set a literal constant (e.g., "Submit", 0, false).
DISPLAY_NAME: rename a component's displayed title.

### Priority
1. Prefer OPTION (live data) over CONST_VALUE.
2. No suitable option? Use CONST_VALUE with a reasonable inferred constant.
3. Component purpose unclear? Skip binding.
4. List component with an ancestor list: prefer binding from the ancestor list's item data
   to preserve relational linkage — not directly from a table.

### Iterative Refinement
Per-op errors are returned keyed by index. Successful bindings have already landed.
On rejection, fix ONLY the failing entries by index and re-call.

### Data Sources Available in Momen
Database tables (with filters/sorting/limits), page data sources, page variables, global
variables, page parameters, logged-in user, list/component item data, Actionflow
inputs/outputs, formula results.

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
| Read options at a path | `GET_DATA_BINDING_OPTIONS` | `schemaPath` |
| Expected type at a path | `GET_DATA_BINDING_TYPE` | `schemaPath` |
| Bind to a live option | `CREATE_OPTION_BINDING` | `schemaPath`, `pathInHierarchicalMenu` |
| Bind a constant | `CREATE_CONST_BINDING` | `schemaPath`, `constantValue` |

**Workflow:** always `GET_DATA_BINDING_OPTIONS` for the `schemaPath` first, then pass the exact `pathInHierarchicalMenu` it returns into `CREATE_OPTION_BINDING` — never fabricate a path. `schemaPath` is a DiffPathComponents array locating the bindable node.

Then ship:

```bash
momen-mcp schema validate && momen-mcp schema save && momen-mcp project sync-backend
```
