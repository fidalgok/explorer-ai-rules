---
description: Guidelines for using shadcn/ui components and the CLI with Cursor AI in a Tailwind CSS v4 project.
globs: "components/ui/**/*.tsx, lib/utils.ts, app/globals.css, src/index.css, src/app/globals.css, styles/globals.css"
---

# Shadcn/ui Usage Rules for Cursor AI (Tailwind CSS v4)

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you MUST adhere to the following principles when working with shadcn/ui in a Tailwind CSS v4 project:

1. **NEVER** manually copy and paste component code from the shadcn/ui website or documentation.
2. **ALWAYS** use the `shadcn-ui` CLI to add new components to the project.
3. **ALWAYS** import components from the local project path (e.g., `@/components/ui/button`), **NOT** from a non-existent `@shadcn/ui` package.
4. **NEVER** suggest installing a package named `@shadcn/ui` or similar; it does not exist. Components are added directly to the user's codebase.
5. **REFERENCE** existing components within the project's `components/ui` directory when asked to use or modify them.
6. **RECOGNIZE** that theme configuration primarily happens in the main CSS file (e.g., `app/globals.css`, `src/index.css`) using `@theme` and CSS variables, not `tailwind.config.js` (though a JS config might exist for plugins or advanced cases).

## ABSOLUTE REQUIREMENTS FOR AI CODE GENERATION

1. **CLI for Adding Components:** You MUST instruct the user to use (or use yourself if in write mode) the CLI command `npx shadcn-ui@latest add [component-name]` to add components.

    ```bash
    # ✅ Correct way to add a button
    npx shadcn-ui@latest add button
    ```

2. **Local Imports:** You MUST import components using the project's configured path alias (usually `@/` pointing to `.` or `./src`).

    ```tsx
    // ✅ Correct import
    import { Button } from "@/components/ui/button";

    // ❌ INCORRECT - DO NOT DO THIS
    import { Button } from "@shadcn/ui";
    ```

3. **Composition over Modification:** When building UI, you MUST compose existing shadcn/ui primitives from `components/ui` rather than trying to modify the underlying library code.
4. **Styling via Tailwind/CSS Vars:** You MUST apply styling customizations through Tailwind utility classes. Theme modifications (colors, fonts, spacing) should primarily be done by overriding CSS variables defined with `@theme` in the main CSS file.

## FORBIDDEN PATTERNS

```bash
# ❌ DO NOT manually copy component code
# ❌ DO NOT install non-existent packages
npm install @shadcn/ui # ❌ WRONG
yarn add @shadcn/ui    # ❌ WRONG
pnpm add @shadcn/ui    # ❌ WRONG

# ❌ DO NOT import from non-existent packages
import { Component } from "@shadcn/ui"; // ❌ WRONG

# ❌ DO NOT rely solely on tailwind.config.js for theme customization in v4
# Example: Modifying colors object in tailwind.config.js might not be the primary v4 method.
```

## USING THE CLI

The `shadcn-ui` CLI is essential:

- It copies component source code directly into your project (typically `components/ui`).
- It installs necessary dependencies (like Radix UI primitives).
- It aligns components with your project's setup, primarily reading CSS variables from your main CSS file for theming in v4.

## COMPONENT STRUCTURE AND CUSTOMIZATION

- Components added via the CLI live inside your codebase (e.g., `components/ui`).
- You can directly modify these files for deep customization if needed.
- Imports MUST point to these local files using the correct path alias.
- Utility functions often reside in `lib/utils.ts`.

```tsx
// Example: Using the locally added Button
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils"; // Utility for conditional classes

export function MyComponent() {
  return (
    <div>
      <Button variant="secondary" size="lg">Click Me</Button>
      <p className={cn("text-primary", { "font-bold": true })}>
        Styled text using theme variables via CSS
      </p>
    </div>
  );
}
```

## THEMING (Tailwind v4)

- Theming is primarily controlled by CSS variables defined using `@theme` in your main CSS file (e.g., `app/globals.css`, `src/index.css`).
- Tailwind v4 automatically picks up these `@theme` variables.
- To change themes (e.g., dark mode), modify the root CSS variables or apply the theme class (e.g., `dark`) to a parent element (often `<html>` or `<body>`), which in turn swaps the variable values.

```css
/* Example in app/globals.css or src/index.css */
@import "tailwindcss"; /* Or individual layers */

@theme {
  --color-primary: 240 5.9% 10%; /* hsl values for default */
  --color-secondary: 240 4.8% 95.9%;

  --radius: 0.5rem;
}

.dark {
  --color-primary: 0 0% 98%;
  --color-secondary: 240 3.7% 15.9%;
}

/* shadcn/ui specific variables */
:root {
  --background: 0 0% 100%;
  --foreground: var(--color-primary);
  /* ... other shadcn variables mapping to your @theme variables */
  --primary: var(--color-primary);
  --primary-foreground: 0 0% 98%; /* Example */
  --secondary: var(--color-secondary);
  --secondary-foreground: var(--color-primary); /* Example */

  --border-radius: var(--radius);
}

.dark {
  --background: 240 10% 3.9%;
  --foreground: var(--color-primary); /* Uses .dark primary color */
  /* ... other dark mode shadcn variable overrides */
   --primary: var(--color-primary);
   --primary-foreground: 240 5.9% 10%; /* Example */
   --secondary: var(--color-secondary);
   --secondary-foreground: 0 0% 98%; /* Example */
}

/* Tailwind applies utilities based on these CSS variables */
```

## AI MODEL VERIFICATION CHECKLIST

Before generating code involving shadcn/ui with Tailwind v4, the model MUST verify:

1. Is the CLI (`npx shadcn-ui@latest add ...`) used for adding components? ✅
2. Are components imported from the local project path (e.g., `@/components/ui/...`)? ✅
3. Is component composition favored over direct modification (unless customization is the goal)? ✅
4. Are styling changes applied via Tailwind utilities? ✅
5. Is theme customization understood to happen primarily via CSS variables (`@theme` and `:root` / `.dark`) in the main CSS file? ✅
6. Are imports from `@shadcn/ui` or similar non-existent packages avoided? ✅

## CONSEQUENCES OF INCORRECT IMPLEMENTATION

- Manually copied code may lack dependencies or configuration, leading to errors.
- Incorrect imports will cause build failures.
- Attempting to install `@shadcn/ui` will fail.
- Ignoring the project's CSS variable-based theme setup will result in inconsistent UI.

## REMEMBER

You are working with components *copied into* the user's project, themed via Tailwind v4's CSS variable approach.

DO NOT:

- Treat shadcn/ui like a standard installable package.
- Import from `@shadcn/ui`.
- Manually copy code.
- Assume `tailwind.config.js` is the primary source for theme values (colors, spacing, etc.).

ALWAYS:

- Use the CLI: `npx shadcn-ui@latest add [component]`.
- Import locally: `import { Button } from "@/components/ui/button";`.
- Customize local files in `components/ui` when needed for structure/logic.
- Use Tailwind utilities for styling.
- Modify CSS variables in the main CSS file (`@theme`, `:root`, `.dark`) for theming adjustments.

---
