---
title: Use Installed Docs as Source of Truth
impact: HIGH
tags: [docs, accuracy, versioning]
---

# Use Installed Docs as Source of Truth

React Router ships markdown docs inside the package. Read them for version-accurate guidance instead of relying on memory or external sources. APIs change between versions — the installed docs always match the installed code.

## Why

- **Version accuracy**: API signatures, exports, and behaviors change across React Router versions
- **Mode correctness**: Each doc includes a mode marker so you only apply relevant guidance
- **Completeness**: Local docs cover edge cases and migration notes that general knowledge misses

## Pattern

Read docs from the installed package:

```txt
node_modules/react-router/docs/
```

Key paths:

```txt
node_modules/react-router/docs/index.md
node_modules/react-router/docs/start/framework/    # Framework Mode
node_modules/react-router/docs/start/data/          # Data Mode
node_modules/react-router/docs/start/declarative/   # Declarative Mode
node_modules/react-router/docs/how-to/              # Task-specific guides
node_modules/react-router/docs/explanation/         # Conceptual docs
node_modules/react-router/docs/upgrading/           # Migration guides
```

### Check the Mode Marker

Most docs include a mode marker near the top:

```txt
[MODES: framework, data, declarative]
```

Only apply a doc when its mode marker matches the app's current mode.

### Fallback Order

```txt
1. node_modules/react-router/docs/     ← Installed version (preferred)
2. repo docs/ directory                ← When working inside the React Router repo
3. Version-matched website docs        ← When local docs are missing
```

## Rules

1. Read `node_modules/react-router/docs/` before writing any React Router code
2. Check the `[MODES: ...]` marker — only apply docs that match the app's mode
3. If local docs are missing, fall back to version-matched website docs — never assume API signatures from memory
4. For RSC, read `react-router/docs/how-to/react-server-components.md` in addition to the base mode docs
