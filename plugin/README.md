# momen-nocode — Claude Code, Codex & Cursor plugin

Generated — do not edit by hand. Skill content comes from `build_scripts/gen-skills.mjs`.
Regenerate with `node build_scripts/gen-skills.mjs`.

The `momen-platform` skill orients Claude Code, Codex, or Cursor on building Momen projects (the
pre-type-system-refactor bundle) and routes to capability sub-documents. The same bundle ships three
host manifests — `.claude-plugin/plugin.json` (Claude Code), `.codex-plugin/plugin.json` (Codex),
and `.cursor-plugin/plugin.json` (Cursor) — over one shared `skills/`, `hooks/`, and `bin/`.
Cursor also consumes the `momen` MCP server declared in `mcp.json`; Claude Code and Codex ignore
it and invoke the CLI via the bin launcher. Its CLI recipes invoke the launcher by absolute
path (`${PLUGIN_ROOT:-${CLAUDE_PLUGIN_ROOT}}/bin/momen-mcp`, which resolves under both hosts)
rather than as a bare command, so a globally-installed `momen-mcp` (or `momen`) on PATH can't
shadow it. The launcher always runs the pinned published CLI via `npx -y momen-mcp@<version>`, so
the plugin behaves identically whether installed from a marketplace or run locally.

## Local use

    # Claude Code
    claude --plugin-dir ./plugin
    /momen-nocode:momen-platform

    # Codex
    codex plugin marketplace add momen-tech-org/momen-nocode-plugin
    codex plugin add momen-nocode@momen

    # Cursor (local dev) — symlink, then enable it in Cursor's plugin settings
    ln -s "$PWD/plugin" ~/.cursor/plugins/local/momen-nocode

The plugin always runs the **published** npm CLI, so local CLI changes are not exercised through it.
To test the CLI itself, run it directly (e.g. `pnpm build && node bin/momen-mcp.js …`).
