---
title: Data Mode Hooks
description: Data router hooks for loaders, actions, fetchers, and pending UI
tags: [hooks, data-mode, useLoaderData, useActionData, useFetcher, useNavigation]
---

# Data Mode Hooks

Use these hooks with data routers (`createBrowserRouter` + `RouterProvider`). The list below only includes hooks that are available in data mode.

## Quick Reference

| Hook | Use When |
| ---- | -------- |
| `useLoaderData` | Read loader data for the current route |
| `useRouteLoaderData` | Read loader data from another route by `id` |
| `useMatches` | Access matched routes, loader data, and `handle` |
| `useActionData` | Read data returned from the current route action |
| `useSubmit` | Submit data programmatically to actions |
| `useFormAction` | Resolve the action URL relative to the route |
| `useFetcher` | Submit or load without navigation |
| `useFetchers` | Observe in-flight fetchers for optimistic UI |
| `useNavigation` | Global pending or submitting state |
| `useRevalidator` | Manually revalidate loader data |
| `useRouteError` | Render errors from loaders, actions, or render |
| `useAsyncValue` | Read resolved values inside `<Await>` |
| `useAsyncError` | Read rejected values inside `<Await>` |

## Loader Data and Matches

```tsx
import { createBrowserRouter, useLoaderData, useRouteLoaderData, useMatches } from "react-router";

const router = createBrowserRouter([
  {
    id: "root",
    path: "/",
    loader: async () => ({ user: await getUser() }),
    element: <Root />,
    children: [
      {
        path: "reports/:reportId",
        loader: async ({ params }) => getReport(params.reportId),
        element: <Report />,
        handle: { breadcrumb: "Report" },
      },
    ],
  },
]);

function Report() {
  const report = useLoaderData();
  const { user } = useRouteLoaderData("root");
  const matches = useMatches();
  const crumb = matches.find((match) => match.handle?.breadcrumb)?.handle.breadcrumb;

  return (
    <div>
      <h1>{crumb}</h1>
      <p>Signed in as {user.name}</p>
      <pre>{report.title}</pre>
    </div>
  );
}
```

## Actions and Submissions

```tsx
import { Form, useActionData, useFormAction, useSubmit } from "react-router";
import { useRef } from "react";

function ProfileForm() {
  const actionData = useActionData();
  const submit = useSubmit();
  const action = useFormAction();
  const formRef = useRef(null);

  return (
    <Form ref={formRef} method="post">
      <input name="displayName" />
      {actionData?.errors?.displayName && <p>{actionData.errors.displayName}</p>}
      <button type="submit">Save</button>
      <button
        type="button"
        onClick={() => formRef.current && submit(formRef.current, { method: "post", action })}
      >
        Save via useSubmit
      </button>
    </Form>
  );
}
```

## Fetchers and Global Fetcher State

```tsx
import { useFetcher, useFetchers } from "react-router";

function LikeButton({ postId, liked }) {
  const fetcher = useFetcher();
  const optimistic = fetcher.formData
    ? fetcher.formData.get("liked") === "true"
    : liked;

  return (
    <fetcher.Form method="post" action={`/posts/${postId}/like`}>
      <input type="hidden" name="liked" value={String(!optimistic)} />
      <button>{optimistic ? "Unlike" : "Like"}</button>
    </fetcher.Form>
  );
}

function FetcherStatus() {
  const fetchers = useFetchers();
  const isBusy = fetchers.some((fetcher) => fetcher.state !== "idle");

  return isBusy ? <span>Updating...</span> : null;
}
```

## Pending UI and Revalidation

```tsx
import { Outlet, useNavigation, useRevalidator } from "react-router";

function Root() {
  const navigation = useNavigation();
  const { revalidate } = useRevalidator();
  const isPending = navigation.state !== "idle";

  return (
    <div>
      <button type="button" onClick={() => revalidate()}>
        Refresh data
      </button>
      {isPending && <span>Loading...</span>}
      <Outlet />
    </div>
  );
}
```

## Route Errors

```tsx
import { isRouteErrorResponse, useRouteError } from "react-router";

function ReportErrorBoundary() {
  const error = useRouteError();

  if (isRouteErrorResponse(error)) {
    return <p>{error.status}: {error.data}</p>;
  }

  if (error instanceof Error) {
    return <p>{error.message}</p>;
  }

  return <p>Unknown error</p>;
}
```

## Await Hooks

```tsx
import { Await, useAsyncError, useAsyncValue, useLoaderData } from "react-router";
import { Suspense } from "react";

function ReportScreen() {
  const { reportPromise } = useLoaderData();

  return (
    <Suspense fallback={<p>Loading report...</p>}>
      <Await resolve={reportPromise} errorElement={<ReportError />}>
        <ReportDetails />
      </Await>
    </Suspense>
  );
}

function ReportDetails() {
  const report = useAsyncValue();
  return <h1>{report.title}</h1>;
}

function ReportError() {
  const error = useAsyncError();
  return <p>{error instanceof Error ? error.message : "Failed to load"}</p>;
}
```
