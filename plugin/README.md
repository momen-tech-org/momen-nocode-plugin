# momen-nocode — Claude Code, Codex & Cursor plugin

Generated — do not edit by hand. Skill content comes from `build_scripts/gen-skills.mjs`.
Regenerate with `node build_scripts/gen-skills.mjs`.

The `momen-platform` skill orients Claude Code, Codex, or Cursor on building Momen projects,
detects each active project's type-system generation, and routes to an isolated matching capability
tree. The same bundle ships three
host manifests — `.claude-plugin/plugin.json` (Claude Code), `.codex-plugin/plugin.json` (Codex),
and `.cursor-plugin/plugin.json` (Cursor) — over one shared `skills/`, `hooks/`, and `bin/`.
Cursor also consumes the `momen` MCP server declared in `mcp.json`; Claude Code and Codex ignore
it. Its CLI recipes invoke the pinned published CLI directly via `npx -y momen-mcp@<version>`, which
runs this plugin's exact build so a globally-installed `momen-mcp` (or `momen`) on PATH can't
shadow it — with no dependency on the host exposing `PLUGIN_ROOT`/`CLAUDE_PLUGIN_ROOT`. The bundled
`bin/momen-mcp` launcher runs the same `npx -y momen-mcp@<version>` and stays available for local shell use.

## Local use

    # Claude Code
    claude --plugin-dir ./plugin
    /momen-nocode:momen-platform

    # Codex
    codex plugin marketplace add momen-tech-org/momen-nocode-plugin
    codex plugin add momen-nocode@momen

    # Cursor (local dev) — symlink, then enable it in Cursor's plugin settings
    ln -s "$PWD/plugin" ~/.cursor/plugins/local/momen-nocode

## Verify project routing

From the plugin directory, load a project once and inspect its exact `typeSystem`:

    ./bin/momen-mcp --no-daemon schema load \
      --args '{"projectExId":"<exId>"}' \
      --pretty

`schema load` does not accept `--projectExId` directly. Alternatively, pin the project with
`./bin/momen-mcp project set-current --projectExId <exId>`, then run
`./bin/momen-mcp schema load --pretty`.

The plugin always runs the **published** npm CLI, so local CLI changes are not exercised through it.
To test the CLI itself, run it directly (e.g. `pnpm build && node bin/momen-mcp.js …`).
