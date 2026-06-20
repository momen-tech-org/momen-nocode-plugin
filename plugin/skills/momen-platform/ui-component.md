# UI components & layout

## Component Domain Knowledge
The UI is a tree of components organized into pages.

### Categories
Layout (container — can hold other components):
- PAGE: root of each page; no parent.
- LAYOUT_VIEW: generic Flexbox container; the most common container.
- MODAL: popup/dialog container.

Special (complex — add children to their built-in slots, NOT directly to the component):
- LIST: repeating list; has a built-in "List Item View" slot.
- TAB_VIEW: tabbed interface; has a built-in view per tab.
- SELECT_VIEW: has built-in selected/unselected state views.
- CONDITIONAL_VIEW: has a built-in view per condition branch.

Leaf/Business (cannot have children):
BUTTON, TEXT, IMAGE, VIDEO, LOTTIE, RICH_TEXT, TEXT_INPUT, NUMBER_INPUT, VIDEO_PICKER,
FILE_PICKER, SWITCH, MAP, MAP_MARKER, CALENDAR, HORIZONTAL_LINE, SHEET, HTML,
RICH_TEXT_EDITOR, MIX_IMAGE_PICKER, DATE_TIME_PICKER, DATA_SELECTOR, PROGRESS_BAR.
MAP_MARKER can only be placed inside MAP.

### Hierarchy Rules
Every component except PAGE and MODAL must have a Layout Component as its parent.
Prefer relative positioning; use absolute/fixed only when truly necessary.

### Layout
Momen uses Flexbox. Layout containers configure: direction (row/column), justify-content
(start, space-between, space-evenly, space-around), gap, wrap, overflow (scroll/visible/hidden).

### Responsive Breakpoints
Phone: 0–767 px | Tablet: 768–1279 px | Desktop: 1280–1920 px.
Primary breakpoint is Desktop; changes propagate unless overridden per breakpoint.

### Component Configuration (right sidebar in editor)
- Design tab: layout, spacing, size, visual styles.
- Data tab: bind data sources, conditional visibility.
- Action tab: event triggers (onClick, onChange, onLoad, etc.).

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
| List pages | `GET_ALL_PAGE_NAMES` | — |
| Component info | `GET_COMPONENT_INFO` | `componentId` |
| Add components | `ADD_COMPONENT` | `items` |
| Update a component | `UPDATE_COMPONENT` | `componentId` |
| Delete components | `DELETE_COMPONENTS` | `componentIds` |

Add children of LIST/TAB_VIEW/SELECT_VIEW/CONDITIONAL_VIEW into their built-in slots, not directly onto the component. Every component except PAGE/MODAL needs a layout container as parent.

Then ship:

```bash
momen-mcp schema validate && momen-mcp schema save && momen-mcp project sync-backend
```
