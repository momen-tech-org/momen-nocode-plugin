# momen-nocode — Claude Code & Codex plugin

Generated — do not edit by hand. Skill content comes from `build_scripts/gen-skills.mjs`.
Regenerate with `node build_scripts/gen-skills.mjs`.

The `momen-platform` skill orients Claude Code or Codex on building Momen projects (the
pre-type-system-refactor bundle) and routes to capability sub-documents. The same bundle ships two
manifests — `.claude-plugin/plugin.json` (Claude Code) and `.codex-plugin/plugin.json` (Codex) —
over one shared `skills/`, `hooks/`, and `bin/`. Its CLI recipes invoke the launcher by absolute
path (`${PLUGIN_ROOT:-${CLAUDE_PLUGIN_ROOT}}/bin/momen-mcp`, which resolves under both hosts)
rather than as a bare command, so a globally-installed `momen-mcp` (or `momen`) on PATH can't
shadow it. The launcher always runs the pinned published CLI via `npx -y momen-mcp@<version>`, so
the plugin behaves identically whether installed from a marketplace or run locally.

## Local use

    # Claude Code
    claude --plugin-dir ./plugin
    /momen-nocode:momen-platform

    # Codex — discovered via the legacy-compatible .claude-plugin/marketplace.json
    # (Codex reads it directly; no separate .agents/ marketplace is shipped)

The plugin always runs the **published** npm CLI, so local CLI changes are not exercised through it.
To test the CLI itself, run it directly (e.g. `pnpm build && node bin/momen-mcp.js …`).
