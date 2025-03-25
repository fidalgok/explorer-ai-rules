---
description: Rules for data loading and server actions in React Router v7 route modules using SSR
globs: "app/**/*.tsx"
---

# React Router v7 Data Loading & Actions Rules for Cursor AI

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you MUST NOT generate any of the following patterns for data loading or server actions:

```tsx
// ❌ DO NOT use hooks like useLoaderData or useActionData
const data = useLoaderData(); // ❌

// ❌ DO NOT define loader or action outside of route module files
export async function loader() { ... } // ❌ IN WRONG CONTEXT
```

Instead, you MUST ALWAYS:
- Define `loader` and `action` inside route module files (e.g., `./routes/product.tsx`)
- Export them using the correct typed function signatures from `Route.LoaderArgs` and `Route.ActionArgs`
- Use `Route.ComponentProps` to access `loaderData` and `actionData`

## ✅ CORRECT PATTERNS FOR DATA LOADING (SSR ONLY)

```tsx
import type { Route } from "./+types/product";

export async function loader({ params }: Route.LoaderArgs) {
  return await db.getProduct(params.pid);
}

export default function Product({ loaderData }: Route.ComponentProps) {
  return <h1>{loaderData.name}</h1>;
}
```

- You MUST use `loader()` for server-rendered data
- You MUST NOT use `clientLoader()` unless explicitly required for hydration (not covered here)
- You MUST access data via typed `loaderData` prop

## ✅ CORRECT PATTERNS FOR SERVER ACTIONS

```tsx
import type { Route } from "./+types/project";

export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const title = formData.get("title");
  return await db.updateProject({ title });
}

export default function Project({ actionData }: Route.ComponentProps) {
  return (
    <Form method="post">
      <input type="text" name="title" />
      <button type="submit">Submit</button>
    </Form>
  );
}
```

- You MUST use `Form` from `react-router` for actions
- You MUST use `Route.ActionArgs` for typing
- You MUST access action result via `actionData` prop

## AI MODEL VERIFICATION CHECKLIST

Before generating any loader or action logic, verify:

1. Are `loader`/`action` declared in the correct file?
2. Are function args typed using `Route.LoaderArgs` or `Route.ActionArgs`?
3. Is data accessed using `loaderData` and `actionData` via `Route.ComponentProps`?
4. Are `useLoaderData` and similar hooks avoided?
5. Is `clientLoader` excluded unless explicitly required?

## CONSEQUENCES OF INCORRECT IMPLEMENTATION

If you generate incorrect patterns:
- Loaders may leak into client bundles
- Components may break type inference
- Data will not be fetched or mutated properly
- Cursor suggestions will regress into deprecated usage

## REMEMBER

DO NOT:
- Use `useLoaderData` or `useActionData`
- Define loaders/actions outside the route module
- Use `clientLoader` unless needed for hydration (not covered here)

ALWAYS:
- Use `Route.ComponentProps` for typed props
- Use `Form` for server mutations
- Export typed `loader` and `action` functions


