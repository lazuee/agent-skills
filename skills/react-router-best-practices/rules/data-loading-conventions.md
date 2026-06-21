---
title: Load Data with Loaders and Actions
impact: HIGH
tags: [data, loaders, actions, forms, fetchers]
---

# Load Data with Loaders and Actions

In Framework and Data modes, load route data with `loader` and mutate with `action`. React Router handles revalidation, error boundaries, and pending states automatically. Ad-hoc `useEffect` fetching bypasses these guarantees.

## Why

- **Automatic revalidation**: React Router revalidates loaders after actions without manual cache invalidation
- **Error handling**: Thrown responses render the route's `ErrorBoundary` automatically
- **Pending UI**: Navigation and submission states are available via `useNavigation` and `useFetcher`
- **SSR compatibility**: Loaders run on the server during SSR; `useEffect` only runs in the browser

## Pattern

### Loaders — Server Data

```tsx
// Framework: route module export
export async function loader({ params }: Route.LoaderArgs) {
  const product = await getProduct(params.productId);
  if (!product) throw data("Not Found", { status: 404 });
  return { product };
}

// Data: route object property
{
  path: "products/:productId",
  loader: async ({ params }) => {
    const product = await getProduct(params.productId);
    if (!product) throw data("Not Found", { status: 404 });
    return { product };
  },
}
```

### Actions — Mutations

```tsx
// Framework: route module export
export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const errors = await validate(formData);
  if (errors) return data({ errors }, { status: 400 });
  await createItem(formData);
  return redirect("/items");
}
```

### Form vs Fetcher

```tsx
// <Form> — mutation that navigates or changes URL
<Form method="post" action="/items">
  <input name="title" />
  <button type="submit">Create</button>
</Form>

// useFetcher — mutation that stays on the same page
const fetcher = useFetcher();
<fetcher.Form method="post" action="/api/toggle-bookmark">
  <input type="hidden" name="id" value={item.id} />
  <button type="submit">{item.bookmarked ? "Unsave" : "Save"}</button>
</fetcher.Form>
```

```txt
# <Form method="get">  → search/filter that updates the URL
# <Form method="post"> → mutation that navigates after completion
# useFetcher           → mutation without navigation (toggle, like, inline edit)
```

## Anti-Patterns

```tsx
// Bad: useEffect fetching in a Framework/Data route component
function Product() {
  const [product, setProduct] = useState(null);
  useEffect(() => {
    fetch(`/api/products/${id}`).then(r => r.json()).then(setProduct);
  }, [id]); // Bypasses revalidation, error boundaries, SSR
}

// Good: Use the route loader
export async function loader({ params }: Route.LoaderArgs) {
  return { product: await getProduct(params.productId) };
}
```

## Rules

1. Use `loader` for data fetching, `action` for mutations — not `useEffect`
2. Throw responses for errors (404, 500) — React Router renders `ErrorBoundary`
3. Return `data(...)` with a status for validation errors — render from `actionData` or `fetcher.data`
4. Use `<Form>` for mutations that navigate; `useFetcher` for same-page mutations
5. Let React Router revalidate after actions — use `shouldRevalidate` only when the default is wrong
6. Parse search params in the loader so the URL is shareable and bookmarkable
