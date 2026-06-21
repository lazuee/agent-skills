---
title: Use Mode-Specific Route Configuration
impact: HIGH
tags: [routing, mode, patterns]
---

# Use Mode-Specific Route Configuration

Each React Router mode configures routes differently. Framework uses route modules and `app/routes.ts`. Data uses route object arrays. Declarative uses JSX `<Routes>`/`<Route>`. Mixing patterns across modes produces code that won't compile.

## Why

- **Type safety**: Framework generates route types from route modules; Data and Declarative do not
- **Correct behavior**: Route loaders, actions, and error boundaries work differently across modes
- **Maintainability**: Consistent patterns make the codebase predictable

## Pattern

### Framework Mode — Route Modules

Framework apps define routes in `app/routes.ts` and implement them as route modules:

```tsx
// app/routes/product.tsx
import type { Route } from "./+types/product";

export async function loader({ params }: Route.LoaderArgs) {
  return { product: await getProduct(params.productId) };
}

export default function Product({ loaderData }: Route.ComponentProps) {
  return <h1>{loaderData.product.name}</h1>;
}
```

```txt
# Route config: app/routes.ts
# Route modules: app/routes/**/*.tsx
# Types: import from ./+types/<route>
# Docs: react-router/docs/start/framework/routing.md
```

### Data Mode — Route Objects

Data apps define routes as object arrays passed to a data router:

```tsx
import { createBrowserRouter, RouterProvider } from "react-router";

const router = createBrowserRouter([
  {
    path: "products/:productId",
    Component: Product,
    loader: async ({ params }) => {
      return { product: await getProduct(params.productId) };
    },
  },
]);
```

```txt
# Router: createBrowserRouter, createHashRouter, createMemoryRouter
# Rendering: <RouterProvider router={router} />
# Types: standard TypeScript — no generated route types
# Docs: react-router/docs/start/data/routing.md
```

### Declarative Mode — JSX Routes

Declarative apps use `<Routes>` and `<Route>` with no data APIs:

```tsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="products/:productId" element={<Product />} />
  </Routes>
</BrowserRouter>
```

```txt
# No: loader, action, <Form>, useFetcher, useNavigation, useLoaderData
# Data: fetch in components with useEffect or a data-fetching library
# Docs: react-router/docs/start/declarative/routing.md
```

## Anti-Patterns

```tsx
// Bad: Using Data Mode route objects in a Framework app
const router = createBrowserRouter([...]); // Framework uses the Vite plugin, not this

// Bad: Using Framework route module exports in a Data app
export async function loader() { ... }  // Data uses loader on route objects, not module exports

// Bad: Adding loaders to a Declarative app
<Route path="about" loader={aboutLoader} element={<About />} /> // Declarative has no loaders
```

## Rules

1. Framework: route modules with typed exports from `./+types/<route>`
2. Data: route object arrays with `loader`/`action` properties
3. Declarative: JSX `<Routes>`/`<Route>` with `element` props — no data APIs
4. Do not mix route patterns across modes
5. Before editing routes, read the mode-specific routing doc from `node_modules/react-router/docs/`
