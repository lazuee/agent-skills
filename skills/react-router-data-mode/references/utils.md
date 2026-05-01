---
title: Data Mode Utilities
description: Utilities that are available with data routers in React Router v7
tags: [utilities, data-mode, data, createRoutesStub, sessions, cookies]
---

# Data Mode Utilities

These utilities are available in data mode. Some are server-only and only apply when a data router is used with a server adapter.

## Quick Reference

| Utility | Use When |
| ------- | -------- |
| `data` | Return JSON plus headers/status from loaders or actions |
| `createRoutesStub` | Test route modules without a real browser router |
| `createStaticHandler` | Run loaders/actions on the server for SSR or pre-rendering |
| `createCookie` | Define a cookie container for session storage |
| `createCookieSessionStorage` | Store sessions in a signed cookie |
| `createMemorySessionStorage` | Use in-memory sessions for tests |
| `createSession` | Create a session object directly (rarely needed) |
| `createSessionStorage` | Build custom session storage strategies |
| `isCookie` | Guard when working with cookie containers |
| `isSession` | Guard when working with session instances |

## data

Use `data` to return a JSON payload with headers or status without manual `Response` construction.

```tsx
import { data } from "react-router";

export async function loader() {
  const todos = await getTodos();
  return data({ todos }, { headers: { "Cache-Control": "max-age=60" } });
}
```

## createRoutesStub

Use route stubs for unit tests and isolated UI previews.

```tsx
import { createRoutesStub } from "react-router";
import { render } from "@testing-library/react";

const RoutesStub = createRoutesStub([
  { path: "/", element: <Dashboard /> },
  { path: "/users/:userId", element: <UserDetail /> },
]);

render(<RoutesStub initialEntries={["/users/123"]} />);
```

## createStaticHandler

Use `createStaticHandler` when you need to execute loaders and actions on the server.

```tsx
import { createStaticHandler } from "react-router";

const handler = createStaticHandler([
  {
    path: "/",
    loader: async () => ({ message: "Hello" }),
    element: <Root />,
  },
]);

const context = await handler.query(request);
```

## Sessions and Cookies

Use cookie-backed or memory-backed session storage in data router server handlers.

```tsx
import {
  createCookie,
  createCookieSessionStorage,
  createMemorySessionStorage,
  data,
  isCookie,
  isSession,
} from "react-router";

const cookie = createCookie("__session", {
  sameSite: "lax",
  secrets: ["super-secret"],
});

if (!isCookie(cookie)) {
  throw new Error("Invalid session cookie");
}

const storage = createCookieSessionStorage({ cookie });
const memoryStorage = createMemorySessionStorage();

export async function action({ request }) {
  const session = await storage.getSession(request.headers.get("Cookie"));

  if (isSession(session)) {
    session.set("flash", "Profile saved");
  }

  return data(
    { ok: true },
    { headers: { "Set-Cookie": await storage.commitSession(session) } },
  );
}
```

If you need a custom persistence strategy, use `createSessionStorage` to wire a custom store (for example, Redis or a database).
