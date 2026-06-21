---
title: Identify the Mode Before Making Changes
impact: CRITICAL
tags: [mode, detection, setup]
---

# Identify the Mode Before Making Changes

Before changing a React Router app, identify whether it uses Framework, Data, or Declarative mode. Each mode has different APIs, file conventions, and available features. Applying patterns from the wrong mode produces code that won't compile or behaves incorrectly.

## Why

- **Correct APIs**: Framework uses route module exports (`loader`, `action`); Data uses route object properties; Declarative has neither
- **File conventions**: Framework expects `app/routes.ts` and route modules under `app/routes/`; Data expects route object arrays; Declarative uses JSX `<Routes>`
- **Type safety**: Framework generates `./+types/<route>` imports; Data and Declarative do not

## Pattern

Check these signals to determine the mode:

### Framework Mode

```txt
# Dependencies
@react-router/dev

# Config files
react-router.config.ts
app/routes.ts
app/entry.server.tsx
app/entry.client.tsx

# Vite plugin (in vite.config.ts)
import { reactRouter } from "@react-router/dev/vite";
plugins: [reactRouter()]
```

### Data Mode

```tsx
// Router creation — no Framework Vite plugin
import { createBrowserRouter, RouterProvider } from "react-router";

const router = createBrowserRouter([
  {
    path: "/",
    Component: Root,
    loader: rootLoader,
    children: [
      { index: true, Component: Home },
    ],
  },
]);

root.render(<RouterProvider router={router} />);
```

```txt
# Also: createHashRouter, createMemoryRouter, createStaticRouter
# Look for: route object arrays, <RouterProvider>, loader/action on objects
# No: react-router.config.ts, @react-router/dev, ./+types/...
```

### Declarative Mode

```tsx
import { BrowserRouter, Routes, Route } from "react-router";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

```txt
# No: loaders, actions, <Form>, useFetcher, useNavigation
# No: data router, route module conventions
```

### RSC Variants

RSC apps exist in both Framework and Data variants. Look for:

```txt
unstable_reactRouterRSC          → RSC Framework Mode
@vitejs/plugin-rsc               → RSC Framework Mode
unstable_RSCRouteConfig          → RSC Data Mode
entry.rsc                        → RSC entry files
"use client" / "server-only"     → client/server boundary markers
```

When RSC is present, also load the RSC rule after loading the base mode rule.

## Rules

1. Check dependencies and config files before assuming a mode
2. Do not apply Framework patterns to a Data app or vice versa
3. RSC is a variant of Framework or Data — identify the base mode first, then layer RSC rules
4. If mode is ambiguous, check for `@react-router/dev` in dependencies (Framework) or `createBrowserRouter` in source (Data)
5. Read the matching reference after identifying the mode
