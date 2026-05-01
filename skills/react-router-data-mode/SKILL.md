---
name: react-router-data-mode
description: Build React applications using React Router's data mode with createBrowserRouter and RouterProvider. Use when working with route objects, loaders, actions, Form, useFetcher, or pending/optimistic UI.
---

# React Router Data Mode

Data mode uses `createBrowserRouter` and `RouterProvider` to enable data loading, actions, and pending UI. This is a client-side SPA - no SSR.

## Project Structure

| File              | Purpose                                |
| ----------------- | -------------------------------------- |
| `src/index.tsx`   | Entry point — renders `RouterProvider` |
| `src/routes.tsx`  | Route definitions with `createBrowserRouter` |
| `src/app.tsx`     | Root layout with `<Outlet />`          |
| `src/pages/*/`    | Page components                        |
| `src/components/protected-route.tsx` | Auth-gated route wrapper |
| `src/lib/auth.tsx` | Auth context/provider used by app root |

## When to Use

- Defining routes in `src/routes.tsx`
- Loading data with `loader` property on routes
- Handling mutations with `action` property
- Navigating with `<Link>`, `<NavLink>`, `<Form>`, `redirect`, `useNavigate`
- Implementing pending/loading UI states with `useNavigation`
- Using `useFetcher` for mutations without navigation

## Router Options

- `createBrowserRouter` for the production app
- `createMemoryRouter` for tests and isolated previews
- `createHashRouter` only when hosting cannot support the history API

## References

Load the relevant reference for detailed guidance:

| Reference                    | Use When                                  |
| ---------------------------- | ----------------------------------------- |
| `references/routing.md`      | Configuring routes, nested routes, layout |
| `references/route-object.md` | Understanding route object properties     |
| `references/data-loading.md` | Loading data with loaders                 |
| `references/actions.md`      | Handling forms, mutations, validation     |
| `references/hooks.md`        | Data router hooks for loaders and actions |
| `references/utils.md`        | Data mode utilities (data, sessions, tests) |
| `references/navigation.md`   | Links, programmatic navigation, redirects |
| `references/pending-ui.md`   | Loading states, optimistic UI             |

## Critical Patterns

### Router Setup (established pattern)

The app uses the `element` prop pattern (JSX) — follow this convention:

```tsx
// src/routes.tsx
import { createBrowserRouter } from "react-router";
import { App } from "./app";
import { HomePage } from "./pages/home";
import { ProtectedRoute } from "./components/protected-route";

export const router = createBrowserRouter([
    {
        path: "/",
        element: <App />,
        children: [
            {
                path: "dashboard",
                element: (
                    <ProtectedRoute>
                        <HomePage />
                    </ProtectedRoute>
                ),
            },
        ],
    },
]);
```

```tsx
// src/index.tsx
import { createRoot } from "react-dom/client";
import { RouterProvider } from "react-router";
import { router } from "./routes";

const root = createRoot(document.getElementById("root")!);
root.render(<RouterProvider router={router} />);
```

```tsx
// src/app.tsx
import "./styles/global.css";
import { Outlet } from "react-router";

export const App = () => {
    return (
        <div className="flex flex-col w-full h-full">
            <Outlet />
        </div>
    );
};
```

### Adding a New Route

1. Create the page component in `src/pages/<name>/index.tsx`
2. Add the route to `src/routes.tsx` under the root `children` array:

```tsx
import { NewPage } from "./pages/new-page";

// Inside the root route's children:
{ path: "new-page", element: <NewPage /> },
```

### Data Loading with Loaders

```tsx
// src/routes.tsx
{
    path: "users",
    element: <UsersPage />,
    loader: async () => {
        const res = await fetch("/api/users");
        return res.json();
    },
},

// src/pages/users/index.tsx
import { useLoaderData } from "react-router";

export const UsersPage = () => {
    const users = useLoaderData();
    return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
};
```

### Forms & Mutations

**Search forms** — use `<Form method="get">`, NOT `onSubmit` with `setSearchParams`:

```tsx
// ✅ Correct
<Form method="get">
  <input name="q" />
</Form>

// ❌ Wrong
<form onSubmit={(e) => { e.preventDefault(); setSearchParams(...) }}>
```

**Inline mutations** — use `useFetcher`, NOT `<Form>` (which causes page navigation):

```tsx
const fetcher = useFetcher();

<fetcher.Form method="post" action={`/api/items/${id}`}>
  <button>Delete</button>
</fetcher.Form>
```

See `references/actions.md` for complete patterns.

### Optimistic UI Pattern

```tsx
function FavoriteButton({ itemId, isFavorite }) {
    const fetcher = useFetcher();
    const optimistic = fetcher.formData
        ? fetcher.formData.get("favorite") === "true"
        : isFavorite;

    return (
        <fetcher.Form method="post" action={`/items/${itemId}/favorite`}>
            <input type="hidden" name="favorite" value={String(!optimistic)} />
            <button>{optimistic ? "★" : "☆"}</button>
        </fetcher.Form>
    );
}
```

See `references/pending-ui.md` for complete patterns.

## Conventions

- Use **`element`** prop with JSX (e.g., `element: <Page />`), not `Component` prop
- Use **relative imports** — no `~` or `@` aliases
- Pages go in `src/pages/<name>/index.tsx`
- The API is proxied at `/api` during dev (Rsbuild proxy → `localhost:5000`)
- Keep auth boundaries explicit with `ProtectedRoute` and shared auth state in `AuthProvider`

## Further Documentation

https://reactrouter.com/start/data
