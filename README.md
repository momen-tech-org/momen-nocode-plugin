# Momen no-code plugin

Build and modify [Momen](https://momen.app) no-code apps from your AI coding assistant — the data
model, UI components, server-side action flows, data bindings, payments, permissions, and runtime
logs — powered by the `momen-mcp` CLI plus guided skills. For Momen projects only.

## Install

### Claude Code

From your terminal:

```bash
claude plugin marketplace add momen-tech-org/momen-nocode-plugin
claude plugin install momen-nocode@momen
```

Or inside Claude Code:

```
/plugin marketplace add momen-tech-org/momen-nocode-plugin
/plugin install momen-nocode@momen
/reload-plugins
```

### Codex

```bash
codex plugin marketplace add momen-tech-org/momen-nocode-plugin
codex plugin add momen-nocode@momen
codex plugin list | grep momen
```

### Cursor

Cursor 2.5+ reads this repo as a plugin marketplace (it ships `.cursor-plugin/marketplace.json`),
installing both the `momen-platform` skill **and** the `momen` MCP server (`mcp.json`):

- **Team / org (Cursor Teams or Enterprise):** an admin adds this Git repo as a team marketplace;
  members then install `momen-nocode` from Cursor's plugin list.
- **Local / development:** symlink the plugin into Cursor's local plugins directory, then enable it
  in Cursor's plugin settings:

```bash
ln -s "$PWD/plugin" ~/.cursor/plugins/local/momen-nocode
```

A public Cursor Marketplace listing is submitted at cursor.com/marketplace/publish (open source,
manually reviewed).

### Other MCP-compatible tools (Windsurf, Cline, Claude Desktop, Qoder, Trae, …)

These don't read the plugin marketplace, but they can use the underlying `momen-mcp` MCP server
directly. Add it to your tool's MCP config:

```json
{
  "mcpServers": {
    "momen": {
      "command": "npx",
      "args": ["-y", "momen-mcp@latest", "mcp"]
    }
  }
}
```

You get Momen's tools (schema, action flows, data bindings, logs, …); the guided `momen-platform`
skill is plugin-only (Claude Code, Codex, Cursor).

## First use

The plugin runs `momen-mcp` through `npx`, so **Node.js 18+** is required. On first use you'll be
asked to authenticate; you can also do it ahead of time:

```bash
npx -y momen-mcp login
```

Then ask your assistant to work on your Momen app — it loads the `momen-platform` skill and takes
it from there.
