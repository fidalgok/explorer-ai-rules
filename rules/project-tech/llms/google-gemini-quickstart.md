---
description: "Google Gemini API quickstart guide with current models and JavaScript implementation"
globs: ["**/*.js", "**/*.ts", "**/*.tsx"]
last_updated: "2025-06-10"
reference_url: "https://ai.google.dev/gemini-api/docs/quickstart"
current_package: "@google/genai"
---

# Google Gemini API Guidelines

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

- Always use the CURRENT package `@google/genai` (not older packages like @google/generative-ai)
- Use LATEST model names (gemini-2.5-pro-preview, gemini-2.5-flash-preview, gemini-2.0-flash)
- Never reference outdated models due to training cutoffs - refer to current models list
- Prefer JavaScript/TypeScript implementation patterns shown in this guide
- Always handle API responses with proper error handling and type safety

## Getting Current Documentation

**⚠️ IMPORTANT**: LLM knowledge cutoffs may contain outdated model names and API patterns. Always reference:

- **Current Models**: https://ai.google.dev/gemini-api/docs/models
- **Quickstart Guide**: https://ai.google.dev/gemini-api/docs/quickstart
- **Text Generation**: https://ai.google.dev/gemini-api/docs/text-generation
- **Structured Output**: https://ai.google.dev/gemini-api/docs/structured-output
- **Function Calling**: https://ai.google.dev/gemini-api/docs/function-calling
- **Files API**: https://ai.google.dev/gemini-api/docs/files
- **Image Understanding**: https://ai.google.dev/gemini-api/docs/image-understanding
- **Audio Understanding**: https://ai.google.dev/gemini-api/docs/audio

## Installation and Setup

### 1. Install Current Package

```bash
npm install @google/genai
```

### 2. Environment Configuration

```bash
# .env.local or .env
GOOGLE_AI_API_KEY=your_api_key_here
```

### 3. Basic Setup

```typescript
import { GoogleGenAI } from "@google/genai";

const ai = new GoogleGenAI({
  apiKey: process.env.GOOGLE_AI_API_KEY,
});
```

## Current Model Selection (As of June 2025)

### ✅ Recommended Models

```typescript
// Premium - Most powerful reasoning
const GEMINI_2_5_PRO = "gemini-2.5-pro-preview";

// Best price-performance balance
const GEMINI_2_5_FLASH = "gemini-2.5-flash-preview";

// Stable production model
const GEMINI_2_0_FLASH = "gemini-2.0-flash";

// Choose based on use case
const getModel = (useCase: "complex" | "balanced" | "production") => {
  switch (useCase) {
    case "complex":
      return GEMINI_2_5_PRO;
    case "balanced":
      return GEMINI_2_5_FLASH;
    case "production":
      return GEMINI_2_0_FLASH;
    default:
      return GEMINI_2_5_FLASH;
  }
};
```

## Key Patterns and Best Practices

### ✅ Basic Text Generation

```typescript
async function generateText(prompt: string) {
  try {
    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash-preview",
      contents: prompt,
    });

    return response.text;
  } catch (error) {
    console.error("Gemini API error:", error);
    throw error;
  }
}

// Usage
const result = await generateText("Explain React hooks in simple terms");
```

### ✅ Structured Output with Schema

```typescript
interface Recipe {
  name: string;
  difficulty: "easy" | "medium" | "hard";
  cookingTime: number;
  ingredients: string[];
}

async function extractRecipe(text: string): Promise<Recipe> {
  const response = await ai.models.generateContent({
    model: "gemini-2.5-flash-preview",
    contents: `Extract recipe information from this text and return as JSON: ${text}`,
    generationConfig: {
      responseMimeType: "application/json",
      responseSchema: {
        type: "object",
        properties: {
          name: { type: "string" },
          difficulty: {
            type: "string",
            enum: ["easy", "medium", "hard"],
          },
          cookingTime: { type: "number" },
          ingredients: {
            type: "array",
            items: { type: "string" },
          },
        },
        required: ["name", "difficulty", "cookingTime", "ingredients"],
      },
    },
  });

  return JSON.parse(response.text) as Recipe;
}
```

### ✅ Function Calling Implementation

```typescript
// Define available functions
const weatherFunction = {
  name: "getCurrentWeather",
  description: "Get current weather for a location",
  parameters: {
    type: "object",
    properties: {
      location: {
        type: "string",
        description: "City name or coordinates",
      },
      unit: {
        type: "string",
        enum: ["celsius", "fahrenheit"],
        description: "Temperature unit",
      },
    },
    required: ["location"],
  },
};

async function chatWithTools(message: string) {
  const response = await ai.models.generateContent({
    model: "gemini-2.5-flash-preview",
    contents: message,
    tools: [{ functionDeclarations: [weatherFunction] }],
  });

  // Handle function calls
  if (response.functionCalls?.length > 0) {
    const functionCall = response.functionCalls[0];

    if (functionCall.name === "getCurrentWeather") {
      // Execute the actual function
      const weatherData = await getCurrentWeather(
        functionCall.args.location,
        functionCall.args.unit
      );

      // Send result back to model
      const followUpResponse = await ai.models.generateContent({
        model: "gemini-2.5-flash-preview",
        contents: [
          { text: message },
          {
            functionResponse: {
              name: functionCall.name,
              response: weatherData,
            },
          },
        ],
        tools: [{ functionDeclarations: [weatherFunction] }],
      });

      return followUpResponse.text;
    }
  }

  return response.text;
}
```

### ✅ Image Understanding

```typescript
async function analyzeImage(imageBase64: string, prompt: string) {
  const response = await ai.models.generateContent({
    model: "gemini-2.5-flash-preview",
    contents: [
      {
        role: "user",
        parts: [
          { text: prompt },
          {
            inlineData: {
              mimeType: "image/jpeg",
              data: imageBase64,
            },
          },
        ],
      },
    ],
  });

  return response.text;
}

// Usage with file upload
async function processUploadedImage(file: File) {
  const base64 = await fileToBase64(file);
  return analyzeImage(base64, "Describe this image in detail");
}
```

### ✅ React Hook Pattern

```typescript
import { useState, useCallback } from "react";
import { GoogleGenAI } from "@google/genai";

export function useGemini() {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const ai = new GoogleGenAI({
    apiKey: process.env.NEXT_PUBLIC_GOOGLE_AI_API_KEY!,
  });

  const generateContent = useCallback(
    async (prompt: string, model: string = "gemini-2.5-flash-preview") => {
      setLoading(true);
      setError(null);

      try {
        const response = await ai.models.generateContent({
          model,
          contents: prompt,
        });

        return response.text;
      } catch (err) {
        const errorMessage =
          err instanceof Error ? err.message : "Unknown error";
        setError(errorMessage);
        throw err;
      } finally {
        setLoading(false);
      }
    },
    [ai]
  );

  return { generateContent, loading, error };
}
```

## Forbidden Patterns

### ❌ Using Outdated Package Names

```typescript
// Wrong: Outdated package
import { GoogleGenerativeAI } from "@google/generative-ai";

// Correct: Current package
import { GoogleGenAI } from "@google/genai";
```

### ❌ Using Outdated Model Names

```typescript
// Wrong: Outdated models (knowledge cutoff references)
const model = "gemini-pro";
const model = "gemini-1.5-pro";

// Correct: Current models (as of Dec 2024)
const model = "gemini-2.5-flash-preview";
const model = "gemini-2.0-flash";
```

### ❌ Missing Error Handling

```typescript
// Wrong: No error handling
const response = await ai.models.generateContent({
  model: "gemini-2.5-flash-preview",
  contents: prompt,
});

// Correct: Proper error handling
try {
  const response = await ai.models.generateContent({
    model: "gemini-2.5-flash-preview",
    contents: prompt,
  });
  return response.text;
} catch (error) {
  console.error("Gemini API error:", error);
  // Handle appropriately
}
```

### ❌ Hardcoded API Keys

```typescript
// Wrong: Hardcoded key
const ai = new GoogleGenAI({ apiKey: "actual-api-key-here" });

// Correct: Environment variable
const ai = new GoogleGenAI({
  apiKey: process.env.GOOGLE_AI_API_KEY,
});
```

## Integration with Common Frameworks

### Next.js API Route

```typescript
// app/api/chat/route.ts
import { GoogleGenAI } from "@google/genai";
import { NextRequest, NextResponse } from "next/server";

const ai = new GoogleGenAI({
  apiKey: process.env.GOOGLE_AI_API_KEY!,
});

export async function POST(request: NextRequest) {
  try {
    const { message } = await request.json();

    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash-preview",
      contents: message,
    });

    return NextResponse.json({ text: response.text });
  } catch (error) {
    return NextResponse.json(
      { error: "Failed to generate content" },
      { status: 500 }
    );
  }
}
```

### Convex Integration

```typescript
// convex/ai.ts
import { v } from "convex/values";
import { action } from "./_generated/server";
import { GoogleGenAI } from "@google/genai";

export const generateWithGemini = action({
  args: {
    prompt: v.string(),
    model: v.optional(v.string()),
  },
  handler: async (ctx, { prompt, model = "gemini-2.5-flash-preview" }) => {
    const ai = new GoogleGenAI({
      apiKey: process.env.GOOGLE_AI_API_KEY!,
    });

    const response = await ai.models.generateContent({
      model,
      contents: prompt,
    });

    return { text: response.text };
  },
});
```

## AI Model Verification Checklist

When implementing Gemini API, verify:

- [ ] Using current package `@google/genai`
- [ ] Using current model names (2.5-pro-preview, 2.5-flash-preview, 2.0-flash)
- [ ] API key stored in environment variables
- [ ] Proper error handling implemented
- [ ] TypeScript types defined for structured outputs
- [ ] Function calling schema properly defined
- [ ] Image/audio data properly encoded for multimodal inputs
- [ ] Rate limiting considerations addressed
- [ ] Security best practices followed (no key exposure)
- [ ] Response validation for structured outputs

## Common Issues and Solutions

### Model Availability

**Problem**: Model not found or deprecated
**Solution**: Check current models documentation and update to latest stable model

### Rate Limiting

**Problem**: API rate limit exceeded
**Solution**: Implement exponential backoff retry logic and consider upgrading API tier

### Large Context Issues

**Problem**: Token limit exceeded
**Solution**: Use models with larger context windows (Gemini 2.0 Flash has 1M tokens) or chunk content

### Structured Output Reliability

**Problem**: Inconsistent JSON responses
**Solution**: Use responseSchema configuration instead of prompt-based instructions

### Function Calling Errors

**Problem**: Functions not called correctly
**Solution**: Ensure function descriptions are detailed and parameter types are specific
