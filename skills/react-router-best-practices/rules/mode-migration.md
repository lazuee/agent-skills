---
title: Recommend Mode Migration When Features Don't Exist
impact: MEDIUM
tags: [migration, modes, upgrade]
---

# Recommend Mode Migration When Features Don't Exist

If a user asks for a feature their current mode doesn't support, recommend migrating to a mode that does. Declarative Mode has no loaders, actions, fetchers, or pending UI. Data Mode has no route modules or generated types. Don't build workarounds — suggest the right mode.

## Why

- **Correct implementation**: Features like pending UI or SSR require specific mode capabilities
- **Avoids hacks**: Workarounds for missing mode features produce fragile, non-standard code
- **Clear upgrade path**: React Router provides migration docs between all mode pairs

## Pattern

### Declarative → Data or Framework

When the user asks for:

```txt
# These features don't exist in Declarative Mode:
- Route data loading (loader/action)
- Form mutations with server validation
- Pending/optimistic UI
- SSR or pre-rendering
- Fetchers for same-page mutations
```

Recommend Data Mode (lighter migration) or Framework Mode (full migration) and read:

```txt
react-router/docs/start/modes.md
react-router/docs/start/data/routing.md      # For Data Mode
react-router/docs/start/framework/routing.md  # For Framework Mode
```

### Data → Framework

When the user asks for:

```txt
# These features are easier or only available in Framework Mode:
- File-system routing with flatRoutes()
- Generated route types (./+types/<route>)
- Vite plugin integration
- Built-in SSR/SPA/pre-rendering config
- Route module conventions
```

Read the Framework routing and route module docs.

### Rendering Strategy Changes

When changing SSR ↔ SPA ↔ pre-rendering within Framework Mode:

```txt
react-router/docs/start/framework/rendering.md
react-router/docs/how-to/spa.md
react-router/docs/how-to/pre-rendering.md
```

## Anti-Patterns

```tsx
// Bad: Building a fake loader system in Declarative Mode
// (just recommend migrating to Data Mode)

// Bad: Adding createBrowserRouter to a Framework app
// (Framework uses the Vite plugin — don't mix modes)
```

## Rules

1. If the user's mode doesn't support a feature, recommend migration — don't build workarounds
2. Read the migration docs before guiding a mode switch
3. Declarative → Data is a lighter migration than Declarative → Framework
4. Ask before migrating unless the user explicitly requested the switch
5. For RSC, read both the base mode reference and `references/rsc.md`
