---
name: momen-platform
description: >-
  Orientation and entry point for working on a Momen project. Load first for any Momen app task — designing or changing the data model, UI, server-side action flows, permissions, payments, or data bindings, or reading runtime logs — then follow it to the right capability. For Momen projects only; not generic programming help.
---

# Momen platform orientation

Before reading platform guidance, identify the active project and pin it when needed:

```bash
npx -y momen-mcp@2.7.2 projects search --projectName "My App"
npx -y momen-mcp@2.7.2 project set-current --projectExId <exId>
npx -y momen-mcp@2.7.2 schema load
```

Read the `typeSystem` field returned by `schema load` exactly, then route without asking the user:

- `pre_type_system_refactor` → read `pre/ROUTER.md`; never read `post/`.
- `post_type_system_refactor` → read `post/ROUTER.md`; never read `pre/`.

If schema loading fails, `typeSystem` is missing, or its value is unknown, stop and report the problem. Never guess a variant or ask the user to choose one.

Repeat this detection whenever the active project changes.

> **Always invoke the CLI exactly as written above** — via `npx -y momen-mcp@2.7.2` — never bare `momen-mcp`, even though `--help` prints its own name that way. The `@2.7.2` version pin runs this plugin's exact published build, so a globally-installed `momen-mcp` (or `momen`) on `PATH` can't shadow it.
