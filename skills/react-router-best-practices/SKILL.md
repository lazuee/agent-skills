---
name: react-router-best-practices
description: Build applications with React Router in Framework, Data, Declarative, and unstable RSC modes. Use when configuring routes, route modules, loaders, actions, forms, fetchers, navigation, pending UI, SSR/SPA/pre-rendering, middleware, URL params/search params, or React Router upgrades.
---

# React Router Best Practices

Conventions for working with React Router across its four modes. Contains 5 rules in 4 categories. Before changing an app, identify the mode, load the matching rule, then read the installed docs for the installed package version.

## When to Apply

- Configuring or editing routes in a React Router app
- Adding data loading (loaders) or mutations (actions) to routes
- Working with forms, fetchers, or pending UI
- Setting up SSR, SPA mode, or pre-rendering
- Migrating between React Router modes
- Debugging route matching, revalidation, or navigation issues

## Rules Summary

### Detection (CRITICAL)

#### identify-mode-first - @rules/identify-mode-first.md

Before changing a React Router app, identify Framework, Data, or Declarative mode from dependencies and config files. Applying patterns from the wrong mode produces code that won't compile.

### Documentation (HIGH)

#### use-installed-docs - @rules/use-installed-docs.md

Read `node_modules/react-router/docs/` for version-accurate guidance. Each doc includes a `[MODES: ...]` marker — only apply docs that match the app's mode.

### Patterns (HIGH)

#### route-configuration - @rules/route-configuration.md

Framework uses route modules with typed exports from `./+types/<route>`. Data uses route object arrays. Declarative uses JSX `<Routes>`/`<Route>`. Do not mix patterns across modes.

#### data-loading-conventions - @rules/data-loading-conventions.md

Load data with `loader`, mutate with `action`. Use `<Form>` for navigation mutations, `useFetcher` for same-page mutations. Don't bypass these with `useEffect` fetching.

### Migration (MEDIUM)

#### mode-migration - @rules/mode-migration.md

If the user's mode doesn't support a feature (e.g., pending UI in Declarative), recommend migrating to Data or Framework Mode. Don't build workarounds.

## Mode Detection Quick Reference

| Mode | Key Signals |
| ---- | ----------- |
| Framework | `@react-router/dev`, `react-router.config.ts`, `app/routes.ts`, `./+types/...` |
| Data | `createBrowserRouter`, `<RouterProvider>`, route object arrays |
| Declarative | `<BrowserRouter>`, `<Routes>`, `<Route element={...}>` |
| RSC | `unstable_reactRouterRSC`, `@vitejs/plugin-rsc`, `unstable_RSCRouteConfig` |

## Philosophy

1. **Mode-first** — Identify the mode before writing any code
2. **Docs-driven** — Read installed docs, not memory
3. **No mixing** — Each mode has its own patterns; don't cross the streams
4. **Migrate, don't hack** — If a mode lacks a feature, recommend upgrading
