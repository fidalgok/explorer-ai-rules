---
description: Guidelines for using React Router v7 route modules in modern app routing with Cursor
globs: "app/routes.ts, app/**/*.tsx"
---

# React Router v7 Routing Rules for Cursor AI

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you MUST NOT generate any of the following outdated or deprecated code patterns:

```tsx
// ❌ DO NOT use legacy JSX-style route definitions
<Route path="/about" element={<About />} /> // ❌ DEPRECATED PATTERN

// ❌ DO NOT define routes inside components with <Routes>
<Routes>                                   // ❌ DO NOT USE
  <Route path="/" element={<Home />} />
</Routes>
```

Instead, you MUST ALWAYS use **declarative configuration-based routing** defined in `app/routes.ts` using `@react-router/dev/routes`:

```ts
// ✅ ALWAYS use this format in app/routes.ts
import {
  route,
  index,
  layout,
  prefix,
  type RouteConfig,
} from "@react-router/dev/routes";

export default [
  index("./home.tsx"),
  route("about", "./about.tsx"),
] satisfies RouteConfig;
```

## ABSOLUTE REQUIREMENTS FOR AI CODE GENERATION

1. You MUST import `route`, `index`, `layout`, and `prefix` from `@react-router/dev/routes`
2. You MUST define routes as an array in `app/routes.ts`
3. You MUST use `satisfies RouteConfig` at the end of the route export
4. You MUST reference route modules by their file path (e.g., `"./about.tsx"`)
5. You MUST use `<Outlet />` for nested routes inside layout modules

## FORBIDDEN PATTERNS

```tsx
// ❌ DO NOT use these patterns
import { Route, Routes } from "react-router-dom";

<Routes>
  <Route path="/" element={<Home />} />
</Routes>

// ❌ DO NOT use createBrowserRouter
createBrowserRouter([...])
```

## NESTED ROUTES AND LAYOUTS

For nested routes, you MUST:
- Use the `layout()` function in `routes.ts`
- Use `<Outlet />` inside the layout component to render children

```ts
// ✅ Correct nested route setup
layout("./dashboard/layout.tsx", [
  index("./dashboard/home.tsx"),
  route("settings", "./dashboard/settings.tsx"),
])
```

```tsx
// ✅ Correct layout module
import { Outlet } from "react-router";

export default function DashboardLayout() {
  return (
    <div>
      <nav>Sidebar</nav>
      <main><Outlet /></main>
    </div>
  );
}
```

## ROUTE MODULES: DOs and DON'Ts

- ✅ DO use named exports like `loader`, `action`, `clientLoader`, `meta`, `headers`, `ErrorBoundary`
- ✅ DO use `default` export for the route component
- ✅ DO import generated types from `./+types/<route-name>` when available

```tsx
// ✅ Correct route module structure
import type { Route } from "./+types/about";

export async function loader() {
  return { title: "About Page" };
}

export default function About({ loaderData }: Route.ComponentProps) {
  return <h1>{loaderData.title}</h1>;
}
```

- ❌ DO NOT use hooks like `useLoaderData`, `useParams`, etc. if typed props are available

## AI MODEL VERIFICATION CHECKLIST

Before generating any code, the model MUST verify:

1. Are routes defined using `@react-router/dev/routes`? ✅
2. Are paths mapped to module file paths (e.g., `"./about.tsx"`)? ✅
3. Is `<Outlet />` used for nested route components? ✅
4. Are route modules exporting a `default` function component? ✅
5. Are `loader`/`action` functions correctly typed and used? ✅
6. Are `route`, `layout`, `prefix`, `index` the only allowed config functions? ✅
7. Is `RouteConfig` used to satisfy the export type? ✅

## CONSEQUENCES OF INCORRECT IMPLEMENTATION

If any of the forbidden patterns are used:
- App will not render routes correctly
- Nested routes will silently fail
- Data loading will not trigger
- Components will not type check
- Routing will break in production

## REMEMBER

You are generating React Router v7 framework-style route config code.

DO NOT:
- Use JSX route definitions
- Use `react-router-dom` route definitions
- Use `createBrowserRouter`

ALWAYS:
- Use file-based module references
- Use config functions like `route()` and `layout()`
- Use `Route.ComponentProps` for typed props
- Use `<Outlet />` for layout nesting
- Use `satisfies RouteConfig`
