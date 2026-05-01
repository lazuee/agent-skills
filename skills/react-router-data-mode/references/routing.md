---
title: Routing
description: Route configuration with createBrowserRouter and route objects
tags: [routing, createBrowserRouter, route-objects, nested-routes, layout, dynamic-segments]
---

# Routing

Routes are configured as JavaScript objects and passed to `createBrowserRouter`.

## Basic Setup

```tsx
import { createBrowserRouter, RouterProvider } from "react-router";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      { index: true, element: <Home /> },
      { path: "about", element: <About /> },
      {
        path: "dashboard",
        element: <Dashboard />,
        children: [
          { index: true, element: <DashboardHome /> },
          { path: "settings", element: <Settings /> },
        ],
      },
    ],
  },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <RouterProvider router={router} />
);
```

## Route Object Properties

| Property   | Purpose                               |
| ---------- | ------------------------------------- |
| `path`     | URL segment to match                  |
| `element`  | React element to render               |
| `children` | Nested routes                         |
| `index`    | Default child route (no path segment) |
| `loader`   | Data loading function                 |
| `action`   | Form submission handler               |

See [route-object.md](./route-object.md) for all properties.

## Nested Routes

Child routes render inside the parent's `<Outlet />`:

```tsx
import { Outlet } from "react-router";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      { path: "teams", element: <Teams /> },
      { path: "projects", element: <Projects /> },
    ],
  },
]);

function Root() {
  return (
    <div>
      <nav>
        <Link to="/teams">Teams</Link>
        <Link to="/projects">Projects</Link>
      </nav>
      <main>
        <Outlet />
      </main>
    </div>
  );
}
```

Parent path is automatically included in children: `/teams` and `/projects`.

### Deeply Nested Routes

```tsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      {
        path: "dashboard",
        element: <Dashboard />,
        children: [
          { index: true, element: <DashboardHome /> },
          { path: "settings", element: <Settings /> },
          { path: "profile", element: <Profile /> },
        ],
      },
    ],
  },
]);
```

Creates: `/dashboard`, `/dashboard/settings`, `/dashboard/profile`.

## Layout Routes

Layout routes have no `path` - they only provide UI wrapper via `<Outlet />`:

```tsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      {
        // No path - this is a layout route
        element: <AuthLayout />,
        children: [
          { path: "login", element: <Login /> },
          { path: "register", element: <Register /> },
        ],
      },
    ],
  },
]);

function AuthLayout() {
  return (
    <div className="auth-container">
      <Outlet />
    </div>
  );
}
```

Both `/login` and `/register` render inside `AuthLayout`.

## Index Routes

Index routes render at the parent's URL (default child):

```tsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      { index: true, element: <Home /> }, // Renders at /
      {
        path: "dashboard",
        element: <Dashboard />,
        children: [
          { index: true, element: <DashboardHome /> }, // Renders at /dashboard
          { path: "settings", element: <Settings /> }, // Renders at /dashboard/settings
        ],
      },
    ],
  },
]);
```

Index routes cannot have children.

## Dynamic Segments

Segments starting with `:` are dynamic and available via `useParams` or `params`:

```tsx
const router = createBrowserRouter([
  {
    path: "/teams/:teamId",
    loader: async ({ params }) => {
      return fetchTeam(params.teamId);
    },
    element: <Team />,
  },
]);

function Team() {
  const { teamId } = useParams();
  const data = useLoaderData();
  return <h1>Team {teamId}: {data.name}</h1>;
}
```

Multiple dynamic segments:

```tsx
{ path: "c/:categoryId/p/:productId", element: <Product /> }
// params: { categoryId: string; productId: string }
```

## Optional Segments

Add `?` to make a segment optional:

```tsx
{ path: ":lang?/categories", element: <Categories /> }
// Matches /categories and /en/categories

{ path: "users/:userId/edit?", element: <User /> }
// Matches /users/123 and /users/123/edit
```

## Splats (Catch-All)

Match any remaining path with `*`:

```tsx
const router = createBrowserRouter([
  {
    path: "/files/*",
    loader: async ({ params }) => {
      const filePath = params["*"]; // e.g., "docs/intro.md"
      return getFile(filePath);
    },
    element: <FileBrowser />,
  },
]);
```

### 404 Catch-All

```tsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      // ... other routes
      {
        path: "*",
        element: <NotFound />,
      },
    ],
  },
]);

function NotFound() {
  return <h1>Page not found</h1>;
}
```

## Anti-Pattern: Flat Routes

```tsx
// ❌ DON'T: Flat structure with no shared layouts
const router = createBrowserRouter([
  { path: "/dashboard", element: <Dashboard /> },
  { path: "/dashboard/settings", element: <DashboardSettings /> },
  { path: "/dashboard/profile", element: <DashboardProfile /> },
]);

// ✅ DO: Use nested routes with shared layout
const router = createBrowserRouter([
  {
    path: "/dashboard",
    element: <DashboardLayout />,
    children: [
      { index: true, element: <Dashboard /> },
      { path: "settings", element: <Settings /> },
      { path: "profile", element: <Profile /> },
    ],
  },
]);
```

## See Also

- [route-object.md](./route-object.md) - All route object properties
- [React Router Routing Documentation](https://reactrouter.com/start/data/routing)
