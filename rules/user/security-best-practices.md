# Security Best Practices for AI Coding Assistants

## Overview

This document establishes critical security guidelines for AI coding assistants to follow when generating code, handling secrets, and implementing authentication/authorization systems.

## 🚨 CRITICAL SECURITY RULES 🚨

### Secret and API Key Management

- ❌ **NEVER:** Write actual API keys, tokens, passwords, or secrets directly in code files
- ❌ **NEVER:** Include real secrets in example code or comments
- ✅ **DO:** Use placeholder patterns like `YOUR_API_KEY_HERE`, `process.env.API_KEY`, or `env.SECRET_NAME`
- ✅ **DO:** Always reference environment variables or secret management systems
- ✅ **DO:** Include clear comments indicating where the human user should add their actual secrets

```typescript
// ✅ CORRECT: Reference environment variable
const apiKey = process.env.GEMINI_API_KEY;

// ✅ CORRECT: Clear placeholder with instructions
const apiKey = "YOUR_API_KEY_HERE"; // Replace with your actual API key from environment variables

// ❌ INCORRECT: Never include real keys
const apiKey = "AIzaSyC-****-****************"; // NEVER DO THIS
```

### Frontend vs Backend Security

- ❌ **NEVER:** Place secrets, API keys, or sensitive configuration in frontend/client-side code
- ❌ **NEVER:** Include database connection strings in client-accessible code
- ✅ **DO:** Keep all sensitive operations on the server/backend
- ✅ **DO:** Use environment variables that are only accessible server-side
- ✅ **DO:** Implement proper API endpoints that handle sensitive operations

### Authentication and Authorization

- ❌ **NEVER:** Generate mock authentication that could be mistaken for production code
- ❌ **NEVER:** Implement authentication with hardcoded user lists or simple checks
- ❌ **NEVER:** Skip authorization checks with comments like "// TODO: Add auth later"
- ✅ **DO:** Implement real authentication systems using established libraries
- ✅ **DO:** Include proper authorization checks for protected resources
- ✅ **DO:** When mocking is necessary for testing, clearly mark it as temporary

```typescript
// ❌ INCORRECT: Mock auth that looks production-ready
function authenticate(username: string, password: string): boolean {
  return username === "admin" && password === "password";
}

// ✅ CORRECT: Real authentication implementation
async function authenticate(
  email: string,
  password: string
): Promise<User | null> {
  const user = await db.user.findUnique({ where: { email } });
  if (!user) return null;

  const isValid = await bcrypt.compare(password, user.hashedPassword);
  return isValid ? user : null;
}

// ✅ CORRECT: Clearly marked temporary mock for testing
// ⚠️ TEMPORARY MOCK - REPLACE WITH REAL AUTH BEFORE PRODUCTION
function mockAuthenticate(username: string): User {
  console.warn("🚨 Using mock authentication - NOT FOR PRODUCTION");
  return { id: "test-user", username, role: "user" };
}
```

### Input Validation and Sanitization

- ✅ **DO:** Always validate and sanitize user inputs
- ✅ **DO:** Use parameterized queries for database operations
- ✅ **DO:** Implement proper CORS policies
- ✅ **DO:** Validate file uploads and restrict file types
- ❌ **NEVER:** Trust user input without validation
- ❌ **NEVER:** Construct SQL queries with string concatenation

### Error Handling and Information Disclosure

- ❌ **NEVER:** Expose sensitive information in error messages
- ❌ **NEVER:** Return stack traces or internal details to clients
- ✅ **DO:** Log detailed errors server-side for debugging
- ✅ **DO:** Return generic error messages to clients
- ✅ **DO:** Implement proper error boundaries

```typescript
// ❌ INCORRECT: Exposes sensitive information
catch (error) {
  return res.status(500).json({
    error: error.message, // Could expose database details, file paths, etc.
    stack: error.stack
  });
}

// ✅ CORRECT: Safe error handling
catch (error) {
  console.error("Database operation failed:", error); // Log internally
  return res.status(500).json({
    error: "An internal error occurred. Please try again later."
  });
}
```

### Data Protection

- ✅ **DO:** Implement proper data encryption for sensitive information
- ✅ **DO:** Use HTTPS for all data transmission
- ✅ **DO:** Implement proper session management
- ✅ **DO:** Follow data minimization principles
- ❌ **NEVER:** Store passwords in plain text
- ❌ **NEVER:** Log sensitive user data

### Dependencies and Package Security

- ✅ **DO:** Recommend using official, well-maintained packages
- ✅ **DO:** Suggest regular dependency updates
- ✅ **DO:** Recommend security scanning tools
- ❌ **NEVER:** Suggest packages with known vulnerabilities
- ❌ **NEVER:** Recommend packages from untrusted sources

## Mock vs Production Code Guidelines

### When Mocking is Acceptable

- Unit tests and test environments
- Development prototypes (clearly marked)
- Educational examples (with explicit warnings)

### Marking Mock Code

Always use clear, prominent markers for temporary/mock code:

```typescript
// 🚨 DEVELOPMENT ONLY - REMOVE BEFORE PRODUCTION
// ⚠️ MOCK IMPLEMENTATION - REPLACE WITH REAL AUTH
// TODO: CRITICAL - Implement real authentication before deployment
// FIXME: Security vulnerability - using mock data
```

### Production Readiness Checklist

Before any authentication/authorization code is considered complete:

- [ ] Real user authentication system implemented
- [ ] Password hashing/encryption in place
- [ ] Session management configured
- [ ] Authorization checks on all protected routes
- [ ] Input validation on all endpoints
- [ ] Error handling doesn't expose sensitive info
- [ ] Security headers configured
- [ ] HTTPS enforced in production

## Configuration Security

- ✅ **DO:** Use environment-specific configuration files
- ✅ **DO:** Keep `.env` files out of version control
- ✅ **DO:** Document required environment variables
- ❌ **NEVER:** Commit configuration files with secrets
- ❌ **NEVER:** Use default or weak configuration values

## General Security Principles

1. **Principle of Least Privilege:** Grant minimum necessary permissions
2. **Defense in Depth:** Implement multiple layers of security
3. **Fail Securely:** Default to denying access when errors occur
4. **Keep Security Simple:** Avoid overly complex security implementations
5. **Regular Updates:** Keep dependencies and systems updated
6. **Security by Design:** Consider security from the start, not as an afterthought

## Human Responsibility

The human user is responsible for:

- Providing actual API keys and secrets through secure means
- Reviewing all generated authentication/authorization code
- Ensuring production deployments meet security requirements
- Configuring proper environment variables and secrets management
- Conducting security reviews before production deployment

## AI Assistant Responsibilities

AI assistants must:

- Never generate or guess real secrets
- Always implement real authentication when requested
- Clearly mark any temporary/mock implementations
- Follow secure coding practices
- Prompt for human review of security-critical code
- Refuse to implement obviously insecure patterns
