---
description: Rules for using fetchers in React Router v7 to handle data interactions without navigation
globs: "app/**/*.tsx"
---

# React Router v7 Fetchers Rules for Cursor AI

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you MUST NOT:

```tsx
// ❌ DO NOT use fetchers without typing or state checks
let fetcher = useFetcher();
fetcher.submit({}) // ❌ Without context or method

// ❌ DO NOT use useLoaderData inside fetcher-based components
// if ComponentProps are available
const data = useLoaderData();
```

Instead, you MUST:

- Use `useFetcher()` from `react-router`
- Submit via `fetcher.Form` or `fetcher.submit`
- Track fetcher state using `fetcher.state`
- Access fetcher results via `fetcher.data`
- Handle optimistic UI using `fetcher.formData`

## ✅ CORRECT PATTERNS FOR FETCHING DATA OR ACTIONS

### Form Submission via Fetcher

```tsx
import { useFetcher } from "react-router";

export default function Component() {
  let fetcher = useFetcher();
  return (
    <fetcher.Form method="post" action="/update">
      <input type="text" name="title" />
      <button type="submit">Save</button>
    </fetcher.Form>
  );
}
```

### Displaying Pending State

```tsx
{fetcher.state !== "idle" && <p>Saving...</p>}
```

### Optimistic UI

```tsx
let title = fetcher.formData?.get("title") || loaderData.title;
```

### Showing Errors or Returned Data

```tsx
{fetcher.data?.error && <p className="error">{fetcher.data.error}</p>}
```

## FETCHING DATA VIA GET REQUEST

```tsx
<fetcher.Form method="get" action="/search-users">
  <input name="q" onChange={(e) => fetcher.submit(e.currentTarget.form)} />
</fetcher.Form>
```

## ✅ VERIFICATION STEPS FOR AI OUTPUT

Before generating fetcher-related code, VERIFY:

1. Are you using `useFetcher()` properly?
2. Are you using `fetcher.Form` or `fetcher.submit()`?
3. Are you checking `fetcher.state` for pending UI?
4. Are you using `fetcher.data` and `fetcher.formData` for dynamic UI?
5. Are you avoiding `useLoaderData()` where `ComponentProps` is provided?

## CONSEQUENCES OF INCORRECT IMPLEMENTATION

If incorrect fetcher usage is generated:

- Component will fail to submit or reflect state properly
- Error handling and optimistic UI will break
- App may perform full navigations unintentionally

## REMEMBER

DO NOT:

- Use `useLoaderData()` where `loaderData` is already a prop
- Submit forms without declaring `method` and `action`
- Omit `fetcher.state` and `fetcher.data` when appropriate
ALWAYS:

- Use `useFetcher()` in fetcher-based UI
- Include pending and error states
- Use `fetcher.Form` or `fetcher.submit` for interaction
