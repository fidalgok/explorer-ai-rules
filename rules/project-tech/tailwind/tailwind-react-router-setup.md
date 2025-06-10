---
description: "Tailwind CSS setup and best practices for React Router v7 projects"
globs: ["**/*.css", "**/*.tsx", "**/*.ts", "vite.config.ts", "tailwind.config.*"]
last_updated: "2024-12-10"
reference_url: "https://tailwindcss.com/docs/installation/framework-guides/react-router"
---

# Tailwind CSS + React Router v7 Guidelines

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

- Always use `@tailwindcss/vite` plugin, NOT the traditional PostCSS setup
- Import Tailwind with `@import "tailwindcss";` in CSS files
- Configure Tailwind plugin BEFORE React Router plugin in vite.config.ts
- Use utility-first approach with semantic component organization
- When custom CSS is needed (especially with shadcn), use proper @layer directives
- Prefer Tailwind utilities but allow strategic custom styles for complex components

## Getting Current Documentation

For the most up-to-date installation and configuration:
- **Primary Source**: https://tailwindcss.com/docs/installation/framework-guides/react-router
- **Functions & Directives**: https://tailwindcss.com/docs/functions-and-directives
- **Utility Classes**: https://tailwindcss.com/docs/styling-with-utility-classes
- **Theme Customization**: https://tailwindcss.com/docs/theme
- **Custom Styles Guide**: https://tailwindcss.com/docs/adding-custom-styles

## Installation and Setup

### 1. Project Creation
```bash
npx create-react-router@latest my-project
cd my-project
```

### 2. Install Tailwind Dependencies
```bash
npm install tailwindcss @tailwindcss/vite
```

### 3. Configure Vite Plugin
In `vite.config.ts`:
```typescript
import { reactRouter } from "@react-router/dev/vite";
import { defineConfig } from "vite";
import tsconfigPaths from "vite-tsconfig-paths";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [
    tailwindcss(), // ⚠️ MUST come before reactRouter()
    reactRouter(),
    tsconfigPaths(),
  ],
});
```

### 4. Import Tailwind CSS and Theme Configuration
In `./app/app.css`:
```css
@import "tailwindcss";

/* Custom theme configuration - preferred approach for design tokens */
@theme {
  /* Custom color palette - prefer neutral/stone grays and custom primary */
  --color-primary-50: oklch(0.95 0.02 264);
  --color-primary-500: oklch(0.55 0.15 264);
  --color-primary-600: oklch(0.48 0.15 264);
  --color-primary-700: oklch(0.42 0.15 264);
  
  /* Override default grays with neutral/stone preference */
  --color-neutral-50: oklch(0.98 0 0);
  --color-neutral-100: oklch(0.96 0 0);
  --color-neutral-500: oklch(0.62 0 0);
  --color-neutral-900: oklch(0.16 0 0);
}

/* Minimal custom components - only when truly necessary */
@layer components {
  /* Complex reusable components that justify custom CSS */
  .card-elevated {
    @apply bg-white border border-neutral-200 rounded-lg p-6 shadow-sm hover:shadow-md transition-all duration-200;
    /* Custom properties that utilities can't handle */
    backdrop-filter: blur(8px);
  }
}
```

## Key Patterns and Best Practices

### ✅ Utility-First Component Design with Preferred Colors
```tsx
// Good: Utility-first with neutral/stone grays and custom primary
export function Button({ children, variant = "primary", ...props }) {
  const baseClasses = "px-4 py-2 rounded-md font-medium transition-colors";
  const variantClasses = {
    primary: "bg-primary-600 hover:bg-primary-700 text-white",
    secondary: "bg-neutral-200 hover:bg-neutral-300 text-neutral-900",
    ghost: "hover:bg-neutral-100 text-neutral-700",
  };
  
  return (
    <button 
      className={`${baseClasses} ${variantClasses[variant]}`} 
      {...props}
    >
      {children}
    </button>
  );
}
```

### ✅ Theme-First Color Usage
```tsx
// Good: Using custom theme colors and neutral grays
export function Card({ children, className }) {
  return (
    <div className={`bg-white border border-neutral-200 rounded-lg p-6 shadow-sm ${className}`}>
      <div className="border-l-4 border-primary-500 pl-4">
        <h3 className="text-lg font-semibold text-neutral-900">Card Title</h3>
        <p className="text-neutral-600 mt-2">{children}</p>
      </div>
    </div>
  );
}
```

### ✅ Responsive Design with Neutral Colors
```tsx
// Good: Mobile-first responsive design with preferred color palette
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <div className="p-4 bg-white border border-neutral-200 rounded-lg shadow-sm hover:shadow-md transition-shadow">
    <h3 className="text-lg font-semibold mb-2 text-neutral-900">Card Title</h3>
    <p className="text-neutral-600 text-sm leading-relaxed">Content</p>
    <button className="mt-3 text-primary-600 hover:text-primary-700 font-medium text-sm">
      Learn more →
    </button>
  </div>
</div>
```

### ✅ Layout with Preferred Color Palette
```tsx
// Good: Semantic layout with neutral colors and custom primary accents
export function Layout({ children }) {
  return (
    <div className="min-h-screen bg-neutral-50">
      <header className="bg-white shadow-sm border-b border-neutral-200">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <nav className="flex justify-between items-center h-16">
            <div className="flex items-center space-x-4">
              <h1 className="text-xl font-semibold text-neutral-900">App Name</h1>
            </div>
            <div className="flex items-center space-x-4">
              <Link 
                to="/dashboard" 
                className="text-neutral-600 hover:text-primary-600 transition-colors"
              >
                Dashboard
              </Link>
            </div>
          </nav>
        </div>
      </header>
      <main className="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
        {children}
      </main>
    </div>
  );
}
```

### ✅ Strategic Custom CSS (Complex Components Only)
```tsx
// Good: When you genuinely need complex styling beyond utilities
export function GlassCard({ children, className }) {
  return (
    <div className={`glass-card ${className}`}>
      {children}
    </div>
  );
}
```

```css
/* @layer components - only for complex, reusable components */
@layer components {
  .glass-card {
    @apply relative bg-white/90 border border-neutral-200/50 rounded-lg p-6 shadow-sm transition-all duration-300;
    
    /* Complex properties that justify custom CSS */
    backdrop-filter: blur(12px) saturate(180%);
    background-image: 
      radial-gradient(circle at 1px 1px, oklch(0.95 0 0 / 0.4) 1px, transparent 0),
      linear-gradient(135deg, transparent 0%, oklch(0.98 0 0 / 0.1) 100%);
    background-size: 16px 16px, 100% 100%;
  }
  
  .glass-card:hover {
    @apply shadow-lg border-primary-200/50;
    transform: translateY(-1px);
    backdrop-filter: blur(16px) saturate(200%);
  }
}
```

### ✅ Minimal @theme Usage (Preferred Approach)
```css
/* Focus primarily on color theming with @theme */
@theme {
  /* Custom primary color - pick one that fits your brand */
  --color-primary-50: oklch(0.95 0.02 264);
  --color-primary-100: oklch(0.90 0.04 264);
  --color-primary-500: oklch(0.55 0.15 264);
  --color-primary-600: oklch(0.48 0.15 264);
  --color-primary-700: oklch(0.42 0.15 264);
  
  /* Prefer neutral over gray for more sophisticated look */
  --color-neutral-50: oklch(0.98 0 0);
  --color-neutral-100: oklch(0.96 0 0);
  --color-neutral-200: oklch(0.93 0 0);
  --color-neutral-300: oklch(0.87 0 0);
  --color-neutral-500: oklch(0.62 0 0);
  --color-neutral-600: oklch(0.52 0 0);
  --color-neutral-700: oklch(0.42 0 0);
  --color-neutral-900: oklch(0.16 0 0);
}
```

## Forbidden Patterns

### ❌ Traditional PostCSS Setup
```javascript
// Wrong: Don't use PostCSS setup with React Router v7
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### ❌ Custom CSS Without @layer
```css
/* Wrong: Custom CSS without proper layer organization */
.custom-button {
  background-color: #3b82f6;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
  /* This can be overridden unexpectedly and doesn't follow Tailwind patterns */
}

/* Better: Use @layer and @apply when custom CSS is needed */
@layer components {
  .custom-button {
    @apply bg-blue-500 px-4 py-2 rounded-md hover:bg-blue-600 transition-colors;
  }
}
```

### ❌ Overusing Custom CSS for Simple Combinations
```css
/* Wrong: Creating custom classes for simple utility combinations */
.flex-center { @apply flex items-center justify-center; }
.text-large { @apply text-xl font-semibold; }
.card-basic { @apply bg-white p-4 rounded shadow; }

/* Better: Use utilities directly in JSX */
```

```tsx
// Wrong: Custom classes for simple combinations
<div className="flex-center">
<h2 className="text-large">
<div className="card-basic">

// Better: Direct utilities (utility-first principle)
<div className="flex items-center justify-center">
<h2 className="text-xl font-semibold text-neutral-900">
<div className="bg-white p-4 rounded shadow border border-neutral-200">
```

### ❌ Using Default Gray Instead of Neutral/Stone
```tsx
// Wrong: Using generic gray colors
<div className="bg-gray-100 text-gray-600 border-gray-200">
<p className="text-gray-900">Content</p>

// Better: Use neutral for more sophisticated appearance
<div className="bg-neutral-100 text-neutral-600 border-neutral-200">
<p className="text-neutral-900">Content</p>
```

### ❌ Inline Styles Over Utilities
```tsx
// Wrong: Don't use inline styles when Tailwind utilities exist
<div style={{ padding: '16px', backgroundColor: '#f3f4f6' }}>
  Content
</div>

// Correct: Use Tailwind utilities
<div className="p-4 bg-gray-100">
  Content
</div>
```

## React Router v7 Specific Considerations

### Route-Level Styling
```tsx
// Good: Route component with Tailwind layout
export default function Dashboard() {
  return (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h1 className="text-2xl font-bold text-gray-900">Dashboard</h1>
        <Link 
          to="/settings" 
          className="text-blue-600 hover:text-blue-700 font-medium"
        >
          Settings
        </Link>
      </div>
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        {/* Dashboard content */}
      </div>
    </div>
  );
}
```

### Loading States with Tailwind
```tsx
// Good: Loading UI with Tailwind animations
export function LoadingSpinner() {
  return (
    <div className="flex justify-center items-center p-8">
      <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
    </div>
  );
}
```

## AI Model Verification Checklist

When implementing Tailwind with React Router, verify:

- [ ] `@tailwindcss/vite` plugin is installed and configured
- [ ] Tailwind plugin comes BEFORE React Router plugin in vite.config.ts
- [ ] CSS imports use `@import "tailwindcss";` syntax
- [ ] Components use utility-first approach with minimal custom CSS
- [ ] Responsive design follows mobile-first methodology
- [ ] Color palette uses Tailwind's semantic color system
- [ ] Spacing follows Tailwind's consistent scale (4, 8, 12, 16, etc.)
- [ ] Typography uses Tailwind's type scale and font weights
- [ ] Interactive states (hover, focus, active) are properly styled
- [ ] Dark mode considerations if applicable (`dark:` variants)

## Common Issues and Solutions

### Plugin Order Error
**Problem**: Styles not applying correctly
**Solution**: Ensure `tailwindcss()` comes before `reactRouter()` in vite.config.ts

### Import Syntax Error  
**Problem**: Tailwind styles not loading
**Solution**: Use `@import "tailwindcss";` instead of traditional PostCSS imports

### Custom CSS Conflicts
**Problem**: Custom styles overriding Tailwind utilities or not being overridable
**Solution**: Always use `@layer` directive to control precedence:
```css
/* Correct layering approach */
@layer base {
  /* Element defaults */
}

@layer components {  
  /* Custom component classes - can be overridden by utilities */
  .btn-custom { @apply px-4 py-2 rounded; }
}

@layer utilities {
  /* Custom utilities - highest precedence */  
  .scroll-smooth { scroll-behavior: smooth; }
}
```

### When to Use Custom CSS with Shadcn
**Valid Use Cases**:
- Complex animations that require keyframes
- Unique visual effects (gradients, patterns, masks)
- Component-specific styles that are reused multiple times
- Overriding deep shadcn component internals
- Creating design system tokens beyond Tailwind's defaults

### Build Size Optimization
**Problem**: Large CSS bundle size
**Solution**: Tailwind automatically purges unused styles in production builds with the Vite plugin