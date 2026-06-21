# momen-nocode — Claude Code plugin

Generated — do not edit by hand. Skill content comes from `build_scripts/gen-skills.mjs`; the
bundled server (`dist/server.mjs`) is copied by the esbuild build. Regenerate with
`node build_scripts/gen-skills.mjs`.

The `momen-platform` skill orients Claude Code on building Momen projects (the
pre-type-system-refactor bundle) and routes to capability sub-documents. Its CLI recipes invoke the
bundled launcher by absolute path (`${CLAUDE_PLUGIN_ROOT}/bin/momen-mcp`) rather than as a bare
command, so a globally-installed `momen-mcp` (or `momen`) on PATH can't shadow the plugin's
pinned build.

## Local use

    pnpm build                  # bundles dist/server.js → plugin/dist/server.mjs
    claude --plugin-dir ./plugin
    /momen-nocode:momen-platform

`dist/` is a build artifact (gitignored); package it (commit or zip) when distributing the plugin.
