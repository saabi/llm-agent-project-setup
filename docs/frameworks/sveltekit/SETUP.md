# SvelteKit Setup Guide

**Framework:** SvelteKit  
**Type:** Full-stack  
**Related:** [Framework Selection Guide](../FRAMEWORK_SELECTION.md) | [LLM Agent Project Setup Guide](../../LLM_AGENT_PROJECT_SETUP.md)

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Project Setup](#project-setup)
4. [Project Structure](#project-structure)
5. [LLM Agent Configuration](#llm-agent-configuration)
6. [TypeScript Setup](#typescript-setup)
7. [Development Workflow](#development-workflow)
8. [Best Practices](#best-practices)
9. [Common Patterns](#common-patterns)
10. [Troubleshooting](#troubleshooting)

---

## Overview

SvelteKit is a full-stack framework built on Svelte that provides excellent performance, small bundle sizes, and a great developer experience. It's particularly well-suited for LLM agent collaboration due to its clear patterns, strong TypeScript support, and predictable structure.

### Key Features

- Server-Side Rendering (SSR) and Static Site Generation (SSG)
- Built-in routing with file-based routing
- API routes for backend functionality
- Excellent performance (compiles to vanilla JavaScript)
- Small bundle sizes
- Strong TypeScript support

### LLM Agent Compatibility

**Rating:** ⭐⭐⭐⭐⭐ Excellent

**Why:**
- Clear, predictable file-based routing
- Strong TypeScript support with excellent type inference
- Consistent patterns across projects
- Well-documented conventions
- Good training data in LLM models (Svelte is well-understood)

---

## Prerequisites

- Node.js 18.x or higher
- npm 9.x or higher (or yarn/pnpm equivalent)

### Verify Installation

```bash
node --version  # Should be 18.x or higher
npm --version   # Should be 9.x or higher
```

---

## Project Setup

### Step 1: Create Project

```bash
npm create svelte@latest my-app
```

**Options to select:**
- **Template:** Skeleton project (or demo for examples)
- **Type checking:** TypeScript (highly recommended for LLM agents)
- **Add ESLint:** Yes (recommended)
- **Add Prettier:** Yes (recommended)
- **Add Playwright:** Optional (for E2E testing)
- **Add Vitest:** Optional (for unit testing)

### Step 2: Install Dependencies

```bash
cd my-app
npm install
```

### Step 3: Verify Setup

```bash
npm run dev
```

Visit `http://localhost:5173` to verify the application is running.

---

## Project Structure

### Recommended Structure (Monorepo Pattern)

```
project-root/
├── docs/                          # Documentation (always at root)
│   ├── specs/                     # System specifications
│   │   ├── feature-specification.md
│   │   ├── technical-architecture.md
│   │   └── development-setup.md
│   ├── tickets/                   # Project tickets
│   └── process/                   # Process documentation
├── app/                           # SvelteKit application
│   ├── src/
│   │   ├── lib/
│   │   │   ├── components/        # Reusable components
│   │   │   └── server/            # Server-only code (API, business logic)
│   │   ├── routes/                # File-based routing
│   │   │   ├── +page.svelte       # Page components
│   │   │   ├── +page.server.ts    # Server load functions
│   │   │   ├── +page.ts           # Universal load functions
│   │   │   ├── api/               # API routes
│   │   │   │   └── [endpoint]/
│   │   │   │       └── +server.ts
│   │   │   └── (group)/           # Route groups
│   │   ├── app.d.ts               # Type definitions
│   │   └── app.html               # HTML template
│   ├── static/                    # Static assets
│   ├── tests/                     # Test files
│   ├── package.json
│   ├── svelte.config.js           # SvelteKit configuration
│   ├── tsconfig.json              # TypeScript config
│   ├── vite.config.ts             # Vite configuration
│   └── README.md
└── README.md                      # Project overview
```

### Key Directories

**`app/src/lib/components/`** - Reusable Svelte components
- Organize by feature or type (e.g., `ui/`, `forms/`, `layout/`)
- Use PascalCase for component files: `Button.svelte`, `UserCard.svelte`

**`app/src/lib/server/`** - Server-only code
- API utilities, database access, authentication
- Business logic that shouldn't run on client
- Use `.server.ts` suffix for clarity

**`app/src/routes/`** - File-based routing
- `+page.svelte` - Page component
- `+page.server.ts` - Server-side data loading
- `+page.ts` - Universal (runs on server and client) data loading
- `+layout.svelte` - Layout wrapper
- `+error.svelte` - Error page
- `api/` - API endpoints

**`app/static/`** - Static assets
- Images, fonts, favicon
- Files served as-is

### SvelteKit-Specific Structure

```
app/src/
├── lib/
│   ├── components/
│   │   ├── ui/                    # UI components (buttons, inputs, etc.)
│   │   ├── forms/                 # Form components
│   │   └── layout/                # Layout components
│   ├── server/
│   │   ├── auth.ts                # Authentication
│   │   ├── db/                    # Database access
│   │   │   └── index.ts
│   │   └── services/              # Business logic services
│   └── utils/                     # Shared utilities (client-safe)
│       └── format.ts
└── routes/
    ├── +layout.svelte             # Root layout
    ├── +page.svelte               # Home page
    ├── about/
    │   └── +page.svelte
    ├── api/
    │   ├── users/
    │   │   └── +server.ts         # GET /api/users
    │   └── auth/
    │       └── login/
    │           └── +server.ts      # POST /api/auth/login
    └── (protected)/               # Route group
        ├── dashboard/
        │   └── +page.svelte
        └── +layout.svelte         # Protected layout
```

---

## LLM Agent Configuration

### .cursorrules Example

```markdown
# Project Rules for LLM Agents

## Framework: SvelteKit

### Project Structure
- Follow SvelteKit conventions
- Components in `app/src/lib/components/`
- Server code in `app/src/lib/server/`
- Routes in `app/src/routes/`
- See: docs/specs/NAMING_CONVENTIONS.md
- See: docs/specs/DEVELOPMENT_SETUP.md

## Code Standards
- Use TypeScript for all new code
- Follow Svelte style guide: https://svelte.dev/docs/development/accessibility-warnings
- Component naming: PascalCase (e.g., `UserCard.svelte`)
- File naming: kebab-case for routes, PascalCase for components
- Use `.server.ts` suffix for server-only code

## SvelteKit-Specific Rules
- Use `+page.svelte` for page components
- Use `+page.server.ts` for server-side data loading
- Use `+page.ts` for universal load functions
- API routes use `+server.ts` files
- Server code must be in `lib/server/` or use `.server.ts` suffix
- Never import server code in client components

## Component Patterns
- Use Svelte stores for state management
- Use `$:` for reactive statements
- Use `<slot>` for component composition
- Prefer props over context when possible

## Commit Guidelines
- Follow Conventional Commits
- Reference tickets in commit messages
- See: docs/process/COMMITS.md
```

### Documentation References

Ensure your `.cursorrules` references:
- Framework selection decision: `docs/decisions/framework-selection.md`
- Naming conventions: `docs/specs/NAMING_CONVENTIONS.md`
- Development setup: `docs/specs/DEVELOPMENT_SETUP.md`
- Commit guidelines: `docs/process/COMMITS.md`

---

## TypeScript Setup

### Configuration

SvelteKit includes TypeScript support out of the box. The `tsconfig.json` is pre-configured.

### Type Definitions

SvelteKit provides excellent type inference:

```typescript
// app/src/routes/+page.server.ts
import type { PageServerLoad } from './$types';

export const load: PageServerLoad = async ({ params, locals }) => {
  // params and locals are fully typed
  return {
    // Return type is inferred
  };
};
```

### App Types

SvelteKit generates types in `.svelte-kit/types/`:

```typescript
// app/src/app.d.ts
import type { User } from '$lib/types';

declare global {
  namespace App {
    interface Locals {
      user: User | null;
    }
    interface PageData {
      // Page data types
    }
    interface Error {
      message: string;
      code?: string;
    }
  }
}
```

---

## Development Workflow

### Starting Development

```bash
npm run dev
```

Runs development server with hot module replacement.

### Building for Production

```bash
npm run build
```

Creates optimized production build in `.svelte-kit/output/`.

### Preview Production Build

```bash
npm run preview
```

### Running Tests

```bash
# Unit tests (if using Vitest)
npm run test

# E2E tests (if using Playwright)
npm run test:e2e
```

### Linting

```bash
npm run lint
```

### Type Checking

```bash
npm run check
```

Runs Svelte's type checker and TypeScript compiler.

---

## Best Practices

### 1. Component Organization

- **Group by feature:** `components/users/UserCard.svelte`
- **Group by type:** `components/ui/Button.svelte`
- **Use index files:** `components/ui/index.ts` for easier imports

### 2. State Management

- **Use Svelte stores** for global state
- **Use props** for component communication
- **Use context** sparingly (only when needed)

```typescript
// app/src/lib/stores/user.ts
import { writable } from 'svelte/store';

export const user = writable<User | null>(null);
```

### 3. Data Loading

- **Use `+page.server.ts`** for server-side data loading
- **Use `+page.ts`** for data needed on both server and client
- **Use `load` functions** for type-safe data fetching

```typescript
// app/src/routes/users/+page.server.ts
import type { PageServerLoad } from './$types';

export const load: PageServerLoad = async ({ locals, fetch }) => {
  const response = await fetch('/api/users');
  const users = await response.json();
  return { users };
};
```

### 4. API Routes

- **Use `+server.ts`** for API endpoints
- **Use proper HTTP methods:** GET, POST, PUT, DELETE
- **Return proper status codes**

```typescript
// app/src/routes/api/users/+server.ts
import { json } from '@sveltejs/kit';
import type { RequestHandler } from './$types';

export const GET: RequestHandler = async ({ locals }) => {
  const users = await getUsers(locals.user);
  return json(users);
};
```

### 5. Error Handling

- **Use `+error.svelte`** for error pages
- **Use `error()` helper** to throw errors
- **Handle errors in load functions**

```typescript
import { error } from '@sveltejs/kit';

export const load: PageServerLoad = async ({ params }) => {
  const user = await getUser(params.id);
  if (!user) {
    throw error(404, 'User not found');
  }
  return { user };
};
```

### 6. Performance

- **Use `+page.ts`** for data that can be loaded on client
- **Use `+page.server.ts`** for sensitive or server-only data
- **Lazy load components** when appropriate
- **Optimize images** using SvelteKit's image handling

---

## Common Patterns

### Pattern 1: Server-Side Data Loading

**Use case:** Loading data that requires authentication or database access

**Example:**
```typescript
// app/src/routes/dashboard/+page.server.ts
import type { PageServerLoad } from './$types';
import { redirect } from '@sveltejs/kit';

export const load: PageServerLoad = async ({ locals }) => {
  if (!locals.user) {
    throw redirect(302, '/login');
  }
  
  const data = await getDashboardData(locals.user.id);
  return { data };
};
```

**LLM Agent Notes:**
- Clear pattern: `+page.server.ts` always for server-side data
- Type-safe with `PageServerLoad`
- Easy for agents to understand and replicate

### Pattern 2: API Endpoint

**Use case:** Creating REST API endpoints

**Example:**
```typescript
// app/src/routes/api/users/+server.ts
import { json } from '@sveltejs/kit';
import type { RequestHandler } from './$types';

export const POST: RequestHandler = async ({ request, locals }) => {
  if (!locals.user) {
    return json({ error: 'Unauthorized' }, { status: 401 });
  }
  
  const data = await request.json();
  const user = await createUser(data);
  return json(user, { status: 201 });
};
```

**LLM Agent Notes:**
- Standard HTTP method handlers
- Type-safe with `RequestHandler`
- Clear error handling pattern

### Pattern 3: Form Handling

**Use case:** Handling form submissions

**Example:**
```svelte
<!-- app/src/routes/users/+page.svelte -->
<script lang="ts">
  import { enhance } from '$app/forms';
  import type { ActionData } from './$types';
  
  export let form: ActionData;
</script>

<form method="POST" use:enhance>
  <input name="name" required />
  <button type="submit">Create User</button>
</form>
```

```typescript
// app/src/routes/users/+page.server.ts
import { fail } from '@sveltejs/kit';
import type { Actions } from './$types';

export const actions: Actions = {
  default: async ({ request }) => {
    const data = await request.formData();
    const name = data.get('name');
    
    if (!name) {
      return fail(400, { error: 'Name is required' });
    }
    
    await createUser(name);
    return { success: true };
  }
};
```

**LLM Agent Notes:**
- Clear separation: form in `.svelte`, action in `.server.ts`
- Type-safe with `Actions` and `ActionData`
- Progressive enhancement with `enhance`

---

## Troubleshooting

### Common Issues

**Issue 1: Server code imported in client component**
- **Error:** "Cannot import server-only module"
- **Solution:** Ensure server code is in `lib/server/` or uses `.server.ts` suffix
- **Check:** Verify imports in client components

**Issue 2: Type errors in load functions**
- **Error:** Type errors in `+page.server.ts` or `+page.ts`
- **Solution:** Run `npm run check` to regenerate types
- **Check:** Ensure `app.d.ts` types are correct

**Issue 3: Routes not working**
- **Error:** 404 on routes
- **Solution:** Check file naming (`+page.svelte`, not `page.svelte`)
- **Check:** Verify route structure matches file structure

### Getting Help

- SvelteKit Documentation: https://kit.svelte.dev/docs
- SvelteKit Discord: https://svelte.dev/chat
- SvelteKit GitHub: https://github.com/sveltejs/kit

---

## Next Steps

1. Review [Framework Selection Guide](../FRAMEWORK_SELECTION.md) if you haven't already
2. Set up project structure following this guide
3. Configure `.cursorrules` for LLM agent collaboration
4. Create initial documentation in `docs/specs/`
5. Follow [LLM Agent Project Setup Guide](../../LLM_AGENT_PROJECT_SETUP.md) for complete project setup

---

## Related Documentation

- [Framework Selection Guide](../FRAMEWORK_SELECTION.md)
- [LLM Agent Project Setup Guide](../../LLM_AGENT_PROJECT_SETUP.md)
- [Project Structure](../../LLM_AGENT_PROJECT_SETUP.md#project-structure)
- [LLM Agent Best Practices](../../LLM_AGENT_PROJECT_SETUP.md#llm-agent-best-practices)
- [SvelteKit Documentation](https://kit.svelte.dev/docs)

---

**Document Version:** 1.0  
**Last Updated:** 2025-01-XX  
**SvelteKit Version:** Latest

