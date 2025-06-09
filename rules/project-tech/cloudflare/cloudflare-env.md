---
# AI Coding Guidelines: React Router v7 on Cloudflare Workers
description: Guidelines for writing server-side TypeScript modules (`*.server.ts`) in React Router v7 applications deployed on Cloudflare, focusing on environment variable access and service initialization.
alwaysApply: true
---

# Guidelines for Server-Side Modules in React Router v7 on Cloudflare

## Getting Current Documentation

When working with Cloudflare, always use the WebFetch or WebSearch tools to access the most recent API documentation and development guidelines:

```
WebFetch URL: https://developers.cloudflare.com/llms.txt
```

This llms.txt file provides direct links to the most current Cloudflare documentation, API references, and development guides.

## When to Fetch Documentation

Use WebFetch with the llms.txt URL when:
- Setting up new Cloudflare Workers projects
- Working with specific Cloudflare APIs (Pages, Workers, D1, R2, KV, etc.)
- Implementing authentication flows
- Configuring environment variables and secrets
- Setting up deployment configurations
- Debugging Cloudflare-specific issues
- Learning about new features or updates

## Overview

This document outlines the critical rules and best practices for AI language models when generating server-side TypeScript code (typically in `*.server.ts` files) for React Router v7 applications running on the Cloudflare Workers platform. The primary focus is on correctly and efficiently accessing environment variables/secrets and initializing service clients (e.g., for external APIs like Google Gemini).

The key objectives are:

1. To avoid "prop-drilling" environment variables or Cloudflare context objects into server utility modules.
2. To correctly handle the Cloudflare Workers runtime I/O restrictions, especially when using globally imported environment variables.
3. To promote clean, maintainable, and performant server-side code.

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you **MUST ADHERE STRICTLY** to the following patterns. Failure to do so will result in code that may break in the Cloudflare Workers environment, be difficult to maintain, or behave unpredictably.

### Environment Variable & Secret Access in `*.server.ts` Files

- ✅ **DO:** Directly import the `env` object from `cloudflare:workers` within your `*.server.ts` files to access any environment variables or secrets needed by that module.

```typescript
// ✅ CORRECT: Direct import in a *.server.ts file
import { env } from 'cloudflare:workers';
const API_KEY = env.GEMINI_API_KEY as string;
// ... use API_KEY
```

- ❌ **NEVER:** Pass the `context.cloudflare.env` object (or individual environment variables from it) as parameters from React Router `loader` or `action` functions into functions within `*.server.ts` utility modules. This is prop-drilling and is what we aim to avoid.

  ```typescript
  // ❌ INCORRECT: Prop-drilling context.cloudflare.env or its properties
  // In your route file (e.g., app/routes/some.route.ts)
  // export async function action({ request, context }: ActionArgs) {
  //   const prompt = "some prompt";
  //   // ❌ Avoid this:
  //   const result = await myUtilityFunction(context.cloudflare.env.GEMINI_API_KEY, prompt);
  //   return { result };
  // }

  // In your utility file (e.g., app/utils/some.server.ts)
  // ❌ Avoid this function signature:
  // export async function myUtilityFunction(apiKey: string, prompt: string) {
  //   //... uses apiKey
  // }
  ```

  **Note:** Accessing `context.cloudflare.env` _directly within the body of a loader or action function itself_ is acceptable if the logic using the environment variable resides entirely within that loader/action. The rule is about not passing it _into separate utility modules_.

### I/O Operations and Service Initialization in `*.server.ts` Files

- ✅ **DO:** Initialize service clients (e.g., API SDKs like `GoogleGenAI`) at the module scope (globally within the `*.server.ts` file) if their constructor **only accepts configuration** (like an API key from `env`) and **DOES NOT perform I/O operations** (like making network requests to connect or fetch initial data).
- ✅ **DO:** Ensure that all actual I/O operations (e.g., calling `model.generateContent()`, `fetch()`, database queries, KV store operations) are performed **only within functions that are called during the request lifecycle** (e.g., exported functions called by loaders/actions, or helper functions called by them).
- ❌ **NEVER:** Perform I/O operations directly in the global/module scope of a `*.server.ts` file, even if you have access to `env` via `import { env } from 'cloudflare:workers';`. This will cause runtime errors in Cloudflare Workers.

  ```typescript
  // ❌ INCORRECT: I/O in global scope
  import { env } from "cloudflare:workers";
  import { SomeApiClient } from "some-api";

  const client = new SomeApiClient(env.API_KEY);
  // ❌ This is an I/O operation and will fail if SomeApiClient.connect() makes a network call here:
  // await client.connect(); // THIS WILL CAUSE AN ERROR AT WORKER INITIALIZATION

  // ❌ Similarly, this would be wrong if it's an I/O call:
  // const initialData = await env.MY_KV_NAMESPACE.get("initial_config"); // ERROR

  export async function handleRequest() {
    //...
  }
  ```

- ❌ **NEVER:** Initialize a service client in the global scope if its constructor _unavoidably performs I/O_. In such cases, use a factory function that creates the client instance within the request lifecycle.

## ABSOLUTE REQUIREMENTS FOR AI CODE GENERATION

1. **Environment Access:** You **MUST** use `import { env } from 'cloudflare:workers';` for accessing environment variables and secrets within `*.server.ts` files.
2. **No Prop-Drilling of `env`:** You **MUST NOT** pass `context.cloudflare.env` or its sub-properties as parameters from loaders/actions into separate server utility functions in `*.server.ts` files.
3. **I/O Operations:** All I/O operations (network calls, database interactions, KV/D1/R2 access, Durable Object method calls) **MUST** be deferred to functions that execute within the request context (i.e., called by a loader or action). Module-level/global scope in `*.server.ts` files is for configuration and non-I/O setup only.
4. **Service Client Initialization:**
    - If a service client's constructor is configuration-only (no I/O), it **SHOULD** be initialized once at the module scope in the `*.server.ts` file using `env` for configuration.
    - If a service client's constructor performs I/O, it **MUST** be initialized within a request-scoped function (e.g., using a factory pattern called from a loader/action).
5. **Secrets Management:**
    - Sensitive data (API keys, tokens) **MUST** be configured as Cloudflare Secrets.
    - For local development, these secrets **MUST** be defined in a `.dev.vars` file.
    - The `.dev.vars` file **MUST ALWAYS** be included in the project's `.gitignore` file.

## CORRECT IMPLEMENTATION EXAMPLE

This example demonstrates the correct way to structure a server utility module for interacting with an external API (like Google Gemini) using an API key from Cloudflare secrets.

**File: `app/ai-utils/gemini.server.ts`**

```typescript
import { env } from "cloudflare:workers";
import { GoogleGenAI, HarmCategory, HarmBlockThreshold } from "@google/genai"; // Or your specific AI SDK

// 1. Access API Key from 'cloudflare:workers' env at module scope
const GEMINI_API_KEY = env.GEMINI_API_KEY as string;

if (!GEMINI_API_KEY) {
  // This check runs at Worker initialization.
  // Throwing an error here will prevent the Worker from starting if the secret is missing.
  throw new Error("CRITICAL: GEMINI_API_KEY environment variable is not set.");
}

// 2. Initialize the AI client at module scope.
// IMPORTANT: The GoogleGenAI constructor here is assumed to ONLY take the API key
// for configuration and NOT perform any network I/O itself.
const genAI = new GoogleGenAI(GEMINI_API_KEY);

// Example model configuration (can also be at module scope if static)
const model = genAI.getGenerativeModel({
  model: "gemini-1.5-flash-latest", // Adjust model as needed
  // System instructions, safety settings etc. can be configured here
  // systemInstruction: "You are a helpful assistant.",
  // safetySettings:,
});

/**
 * Generates content based on a user prompt.
 * This function performs I/O (calls the Gemini API) and is called from a loader/action.
 */
export async function getGeminiResponse(
  userPrompt: string,
  systemInstruction?: string
): Promise<string> {
  console.log("Calling Gemini API...");
  try {
    // If systemInstruction is dynamic per call, configure it here or use startChat
    const currentModel = systemInstruction
      ? genAI.getGenerativeModel({
          model: "gemini-1.5-flash-latest",
          systemInstruction,
        })
      : model; // Use pre-configured model if no dynamic system instruction

    // 3. Perform I/O operation (API call) within this request-scoped function
    const result = await currentModel.generateContent(userPrompt);
    const response = result.response;
    console.log("Gemini API call successful.");
    return response.text();
  } catch (error) {
    console.error("Error calling Gemini API:", error);
    // Handle error appropriately, maybe throw a custom error or return a fallback
    throw new Error("Failed to get response from Gemini API.");
  }
}

// Add other related server functions here, e.g., for prompt evaluation, multi-step processes, etc.
// All functions performing I/O must be designed to be called within a request context.
```

**File: `app/routes/ai.example.ts` (React Router Route Action)**

```typescript
import { ActionFunctionArgs, data } from "react-router";
import { getGeminiResponse } from "~/ai-utils/gemini.server"; // Adjust path as needed

export async function action({ request, context }: ActionFunctionArgs) {
  const formData = await request.formData();
  const userPrompt = formData.get("prompt") as string;
  const customSystemPrompt = formData.get("systemPrompt") as string | undefined;

  if (!userPrompt) {
    return data({ error: "Prompt is required." }, { status: 400 });
  }

  try {
    // Call the server utility function. Notice no env or API key is passed.
    const aiResponse = await getGeminiResponse(userPrompt, customSystemPrompt);
    return data({ success: true, response: aiResponse });
  } catch (error) {
    console.error("Error in AI action:", error);
    return data({ error: "Failed to process your request." }, { status: 500 });
  }
}
```

## AI MODEL VERIFICATION STEPS

Before generating or modifying server-side code for this project, you **MUST** verify:

1. **Environment Access:**
    - Is `import { env } from 'cloudflare:workers';` used in `*.server.ts` files for accessing environment variables/secrets? (✅ **Correct**)
    - Is `context.cloudflare.env` (or its properties) being passed as function arguments from loaders/actions into separate `*.server.ts` utility modules? (❌ **Incorrect** - Stop and Fix)
2. **I/O Operations:**
    - Are all I/O operations (e.g., `fetch`, SDK calls like `model.generateContent()`, database queries, KV/D1/R2 operations) strictly confined to functions that are executed _during_ a request (i.e., exported functions called by loaders/actions, or helpers called by them)? (✅ **Correct**)
    - Are there any I/O operations in the global/module scope of any `*.server.ts` file? (❌ **Incorrect** - Stop and Fix, this will cause runtime errors)
3. **Service Client Initialization:**
    - If a service client (e.g., `GoogleGenAI`) is initialized at the module scope, is its constructor **guaranteed** to be configuration-only (i.e., it does not perform any network I/O or other restricted operations)? (✅ **Correct**)
    - If a service client's constructor _does_ perform I/O, is it being initialized inside a factory function that is only called during the request lifecycle? (✅ **Correct for that specific case**)
    - Is a service client whose constructor performs I/O being initialized at the module scope? (❌ **Incorrect** - Stop and Fix)
4. **Secrets Handling:**
    - Are sensitive values like API keys being treated as secrets (configured in Cloudflare Secrets and `.dev.vars`)? (✅ **Correct**)
    - Is `.dev.vars` present in the `.gitignore` file? (✅ **Correct**)

## CONSEQUENCES OF INCORRECT IMPLEMENTATION

If you generate code that violates these rules, the application may:

1. **Fail to Deploy/Run on Cloudflare Workers:** Due to I/O operations in the global scope.
2. **Be Hard to Maintain:** Due to prop-drilling of environment variables.
3. **Encounter Unexpected Errors:** If service clients are not initialized correctly according to the Workers runtime constraints.
4. **Potentially Leak Secrets:** If `.dev.vars` is not gitignored or secrets are handled improperly (though `import { env }` itself is secure for accessing secrets).

Remember: Adherence to these rules is crucial for a stable, secure, and maintainable application on Cloudflare Workers. There are **NO EXCEPTIONS** to these critical instructions.
