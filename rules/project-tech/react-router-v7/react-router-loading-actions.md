# React Router v7 Data Loading & Actions Rules for Cursor AI

Rules for data loading and server actions in React Router v7 route modules using SSR

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you MUST NOT generate any of the following patterns for data loading or server actions:

```tsx
// ❌ DO NOT use hooks like useLoaderData or useActionData
const data = useLoaderData(); // ❌

// ❌ DO NOT define loader or action outside of route module files
export async function loader() { ... } // ❌ IN WRONG CONTEXT

// ❌ DO NOT use the deprecated json() helper
import { json } from 'react-router'; // ❌ DEPRECATED
return json({ data }, { status: 200 }); // ❌ DEPRECATED
```

Instead, you MUST ALWAYS:

- Define `loader` and `action` inside route module files (e.g., `./routes/product.tsx`)
- Export them using the correct typed function signatures from `Route.LoaderArgs` and `Route.ActionArgs`
- Use `Route.ComponentProps` to access `loaderData` and `actionData`
- Use the `data` helper (not `json`) when you need to set status codes or headers

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
import { data } from "react-router"; // ✅ Import data helper

export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const title = formData.get("title");
  
  if (!title) {
    // ✅ Use data helper for status codes
    return data(
      { message: "Title is required" },
      { status: 400 }
    );
  }
  
  const project = await db.updateProject({ title });
  // Default status is 200, no need for data helper
  return project;
}

export default function Project({ actionData }: Route.ComponentProps) {
  return (
    <Form method="post">
      <input type="text" name="title" />
      <button type="submit">Submit</button>
      {actionData?.message && <p className="error">{actionData.message}</p>}
    </Form>
  );
}
```

- You MUST use `Form` from `react-router` for actions
- You MUST use `Route.ActionArgs` for typing
- You MUST access action result via `actionData` prop
- You MUST use the `data` helper (not `json`) for status codes and headers

## ✅ USING THE DATA HELPER FOR STATUS CODES AND HEADERS

```tsx
import { data } from "react-router"; // ✅ Correct import

export async function loader({ params }: Route.LoaderArgs) {
  const record = await db.getRecord(params.id);
  
  if (!record) {
    // ✅ Throw data for 404 responses
    throw data(null, { status: 404 });
  }
  
  // ✅ Use data helper to set headers
  return data(record, {
    headers: {
      "Cache-Control": "max-age=300"
    }
  });
}
```

- You MUST use the `data` helper from `react-router` for status codes and headers
- You MUST NOT use the deprecated `json` helper
- You can throw `data` to render error boundaries with specific status codes

## AI MODEL VERIFICATION CHECKLIST

Before generating any loader or action logic, verify:

1. Are `loader`/`action` declared in the correct file?
2. Are function args typed using `Route.LoaderArgs` or `Route.ActionArgs`?
3. Is data accessed using `loaderData` and `actionData` via `Route.ComponentProps`?
4. Are `useLoaderData` and similar hooks avoided?
5. Is `clientLoader` excluded unless explicitly required?
6. Is the `data` helper used (not `json`) when setting status codes or headers?

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
- Use the deprecated `json` helper from react-router

ALWAYS:

- Use `Route.ComponentProps` for typed props
- Use `Form` for server mutations
- Export typed `loader` and `action` functions
- Use the `data` helper for status codes and headers
