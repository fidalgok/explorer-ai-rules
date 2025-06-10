# AI Rules Discovery System

*Last updated: 2024-12-10*

This directory contains technology-specific rules and guidelines designed to help AI coding assistants provide better, more accurate assistance. As an AI assistant, use this README to quickly understand which rules apply to the current project and load the appropriate context.

## 🤖 For AI Assistants: Quick Start

### Automatic Technology Detection

1. **Scan the project's `package.json`** to identify installed dependencies
2. **Check this technology matrix** to determine which rules apply
3. **Load relevant rule files** into your working context
4. **Reference the verification checklists** in each rule file

### Manual Technology Specification

When a user says something like:
> "I'm working on a React Router app with Tailwind, Convex DB, and third-party auth"

Load the corresponding rule files and adjust your responses accordingly.

## Technology Coverage Matrix

| Technology | Status | Rule Files | Package.json Indicators | Priority |
|------------|--------|------------|------------------------|----------|
| **React Router v7** | ✅ Complete | [3 files](./project-tech/react-router-v7/) | `@react-router/*` | High |
| **Tailwind CSS** | ✅ Complete | [1 file](./project-tech/tailwind/) | `tailwindcss`, `@tailwindcss/vite` | High |
| **Convex** | ✅ Complete | [1 file](./project-tech/convex/) | `convex` | High |
| **Shadcn/UI** | ✅ Complete | [1 file](./project-tech/shadcn/) | `@radix-ui/*`, `class-variance-authority` | High |
| **Cloudflare** | ✅ Complete | [1 file](./project-tech/cloudflare/) | `@cloudflare/workers-types`, `wrangler` | Medium |
| **Netlify** | ✅ Complete | [1 file](./project-tech/netlify/) | `@netlify/*`, `netlify-cli` | Medium |
| **Firebase** | ❌ Planned | 0 files | `firebase`, `@firebase/*` | Medium |
| **Google Gemini** | ✅ Complete | [1 file](./project-tech/llms/) | `@google/genai` | Medium |
| **LLMs/AI** | 🔄 In Progress | 1 file | `openai`, `@anthropic-ai/*` | Medium |

## Quick Access by Use Case

### Frontend Development
- **React Router + Tailwind**: Most common stack
  - Load: [`react-router-routing.md`](./project-tech/react-router-v7/react-router-routing.md)
  - Load: [`tailwind-react-router-setup.md`](./project-tech/tailwind/tailwind-react-router-setup.md)
  - Load: [`react-router-loading-actions.md`](./project-tech/react-router-v7/react-router-loading-actions.md)

### Full-Stack Development  
- **React Router + Convex + Tailwind**: Complete modern stack
  - Load: All React Router rules + Tailwind + [`convex-guidelines.md`](./project-tech/convex/convex-guidelines.md)

### UI Development
- **Shadcn/UI Components**: When building component libraries
  - Load: [`shadcn-guidelines.md`](./project-tech/shadcn/shadcn-guidelines.md)
  - Also load: Tailwind rules (Shadcn depends on Tailwind)

### Deployment
- **Cloudflare Pages/Workers**: For edge deployment
  - Load: [`cloudflare-env.md`](./project-tech/cloudflare/cloudflare-env.md)
- **Netlify**: For JAMstack deployment  
  - Load: [`netlify-guidelines.md`](./project-tech/netlify/netlify-guidelines.md)

## Essential Rules for All Projects

Always load these core guidelines regardless of technology stack:

1. **[Security Best Practices](./user/security-best-practices.md)** - Critical security guidelines
2. **[Coding Assistant Guidelines](./user/coding-assistant.md)** - Core programming principles
3. **[Feature Development Process](./project-tech/feature-development/prd-instructions.md)** - Development workflow

## Rule File Structure

Each technology rule file follows this structure:
```markdown
---
description: "Brief technology description"
globs: ["file patterns where rules apply"]
last_updated: "YYYY-MM-DD"
reference_url: "link to official docs"
---

# Technology Guidelines

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨
[Key points AI must absolutely follow]

## Getting Current Documentation
[How to access latest docs - llms.txt URLs when available]

## Key Patterns and Best Practices
[Core implementation patterns with examples]

## Forbidden Patterns  
[What NOT to do with examples]

## AI Model Verification Checklist
[Checklist for AI to verify correct implementation]
```

## Package.json Detection Patterns

Use these patterns to auto-detect technologies:

```javascript
// Example detection logic for AI assistants
const detectTechnologies = (packageJson) => {
  const deps = { ...packageJson.dependencies, ...packageJson.devDependencies };
  const detected = [];
  
  if (deps['@react-router/dev'] || deps['react-router-dom']) {
    detected.push('react-router-v7');
  }
  
  if (deps['tailwindcss'] || deps['@tailwindcss/vite']) {
    detected.push('tailwind');
  }
  
  if (deps['convex']) {
    detected.push('convex');
  }
  
  if (deps['@radix-ui/react-slot'] || deps['class-variance-authority']) {
    detected.push('shadcn');
  }
  
  if (deps['@google/genai']) {
    detected.push('google-gemini');
  }
  
  // Add more detection patterns...
  return detected;
};
```

## Integration Examples

### Example 1: New React Router + Tailwind Project
```bash
# User command
"Set up your memory for a new React Router project with Tailwind"

# AI should load:
- rules/project-tech/react-router-v7/react-router-routing.md
- rules/project-tech/react-router-v7/react-router-loading-actions.md  
- rules/project-tech/tailwind/tailwind-react-router-setup.md
- rules/user/security-best-practices.md
- rules/user/coding-assistant.md
```

### Example 2: Full-Stack Convex Project
```bash
# Package.json contains: convex, @react-router/dev, tailwindcss
# AI should auto-load:
- All React Router rules
- Tailwind rules  
- Convex guidelines
- Core user rules
```

## Contributing New Rules

When adding new technology rules:

1. **Follow the standard template** structure above
2. **Add detection patterns** to this README
3. **Update the technology matrix** 
4. **Include practical examples** and forbidden patterns
5. **Add verification checklist** for AI assistants
6. **Reference official documentation** with llms.txt URLs when available

## Troubleshooting

### Rules Not Loading
- Check package.json for correct dependency names
- Verify technology matrix mappings are accurate
- Ensure rule files exist and are properly formatted

### Conflicting Guidelines
- Technology-specific rules override general guidelines
- Security rules always take precedence
- When in doubt, prefer explicit over implicit patterns

## Feedback and Updates

This discovery system is designed to evolve. When working on projects:

1. **Note missing rules** for technologies you encounter
2. **Update existing rules** when patterns change
3. **Add new detection patterns** for emerging tools
4. **Improve verification checklists** based on common issues

The goal is to make AI assistants more effective by providing them with the right context automatically, reducing setup time and improving code quality from the start.