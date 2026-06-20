# momen-nocode — Claude Code plugin

Generated — do not edit by hand. Skill content comes from `build_scripts/gen-skills.mjs`; the
bundled server (`dist/server.mjs`) is copied by the esbuild build. Regenerate with
`node build_scripts/gen-skills.mjs`.

The `momen-platform` skill orients Claude Code on building Momen projects (the
pre-type-system-refactor bundle) and routes to capability sub-documents. The `momen-mcp` binary is
placed on PATH via `bin/`, so the skill's CLI recipes run as-is.

## Local use

    pnpm build                  # bundles dist/server.js → plugin/dist/server.mjs
    claude --plugin-dir ./plugin
    /momen-nocode:momen-platform

`dist/` is a build artifact (gitignored); package it (commit or zip) when distributing the plugin.
