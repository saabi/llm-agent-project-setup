# Next.js Setup Guide

**Framework:** Next.js  
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

Next.js is a full-stack React framework that provides excellent developer experience, extensive ecosystem, and powerful features for building production-ready applications. It's particularly well-suited for LLM agent collaboration due to React's extensive training data in LLM models, clear patterns, and strong TypeScript support.

### Key Features

- Server-Side Rendering (SSR) and Static Site Generation (SSG)
- Built-in routing with file-based routing
- API routes for backend functionality
- Image optimization and performance features
- Extensive React ecosystem
- Strong TypeScript support

### LLM Agent Compatibility

**Rating:** ⭐⭐⭐⭐⭐ Excellent

**Why:**
- React is extensively trained in LLM models
- Abundant examples and patterns in training data
- Clear component structure
- Strong TypeScript support
- Excellent documentation
- Large community and ecosystem

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
npx create-next-app@latest my-app
```

**Options to select:**
- **TypeScript:** Yes (highly recommended for LLM agents)
- **ESLint:** Yes (recommended)
- **Tailwind CSS:** Optional (UI styling)
- **`src/` directory:** Yes (recommended for organization)
- **App Router:** Yes (recommended, modern approach)
- **Import alias:** `@/*` (recommended)

### Step 2: Install Dependencies

```bash
cd my-app
npm install
```

### Step 3: Verify Setup

```bash
npm run dev
```

Visit `http://localhost:3000` to verify the application is running.

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
├── app/                           # Next.js application
│   ├── src/
│   │   ├── app/                   # App Router (Next.js 13+)
│   │   │   ├── layout.tsx         # Root layout
│   │   │   ├── page.tsx           # Home page
│   │   │   ├── (routes)/         # Route groups
│   │   │   │   ├── about/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── dashboard/
│   │   │   │       └── page.tsx
│   │   │   └── api/               # API routes
│   │   │       └── [endpoint]/
│   │   │           └── route.ts
│   │   ├── components/            # React components
│   │   │   ├── ui/                # UI components
│   │   │   ├── forms/            # Form components
│   │   │   └── layout/           # Layout components
│   │   ├── lib/                  # Utilities and server code
│   │   │   ├── server/          # Server-only code
│   │   │   │   ├── auth.ts
│   │   │   │   ├── db/          # Database access
│   │   │   │   └── services/    # Business logic
│   │   │   └── utils/           # Shared utilities
│   │   ├── types/               # TypeScript types
│   │   └── styles/              # Global styles
│   ├── public/                   # Static assets
│   ├── package.json
│   ├── next.config.js           # Next.js configuration
│   ├── tsconfig.json             # TypeScript config
│   └── README.md
└── README.md                     # Project overview
```

### Key Directories

**`app/src/app/`** - App Router (Next.js 13+)
- `layout.tsx` - Layout components
- `page.tsx` - Page components
- `loading.tsx` - Loading UI
- `error.tsx` - Error UI
- `api/` - API route handlers

**`app/src/components/`** - React components
- Organize by feature or type (e.g., `ui/`, `forms/`, `layout/`)
- Use PascalCase for component files: `Button.tsx`, `UserCard.tsx`

**`app/src/lib/server/`** - Server-only code
- API utilities, database access, authentication
- Business logic that shouldn't run on client
- Use `'use server'` directive for server actions

**`app/src/lib/utils/`** - Shared utilities
- Client-safe utility functions
- Helper functions used in components

**`app/public/`** - Static assets
- Images, fonts, favicon
- Files served as-is from `/`

### Next.js-Specific Structure

#### App Router (Next.js 13+, Recommended)

```
app/src/app/
├── layout.tsx                    # Root layout
├── page.tsx                      # Home page
├── loading.tsx                   # Loading UI
├── error.tsx                     # Error UI
├── about/
│   └── page.tsx
├── dashboard/
│   ├── layout.tsx                # Nested layout
│   ├── page.tsx
│   └── settings/
│       └── page.tsx
├── api/
│   ├── users/
│   │   └── route.ts              # GET/POST /api/users
│   └── auth/
│       └── login/
│           └── route.ts          # POST /api/auth/login
└── (protected)/                  # Route group
    ├── profile/
    │   └── page.tsx
    └── layout.tsx                # Protected layout
```

#### Pages Router (Legacy, Still Supported)

```
app/src/pages/
├── _app.tsx                      # App wrapper
├── _document.tsx                 # HTML document
├── index.tsx                     # Home page
├── about.tsx
├── dashboard/
│   └── index.tsx
└── api/
    ├── users.ts                  # API route
    └── auth/
        └── login.ts
```

**Note:** App Router is recommended for new projects. Pages Router is still supported but considered legacy.

---

## LLM Agent Configuration

### .cursorrules Example

```markdown
# Project Rules for LLM Agents

## Framework: Next.js (App Router)

### Project Structure
- Follow Next.js App Router conventions
- Components in `app/src/components/`
- Server code in `app/src/lib/server/`
- Routes in `app/src/app/`
- See: docs/specs/NAMING_CONVENTIONS.md
- See: docs/specs/DEVELOPMENT_SETUP.md

## Code Standards
- Use TypeScript for all new code
- Follow React best practices
- Component naming: PascalCase (e.g., `UserCard.tsx`)
- File naming: kebab-case for routes, PascalCase for components
- Use `'use server'` directive for server actions
- Use `'use client'` directive for client components

## Next.js-Specific Rules
- Use App Router (not Pages Router) for new code
- Server Components by default (no 'use client' unless needed)
- Use Server Actions for form submissions
- API routes use `route.ts` files in `app/api/`
- Server code must be in `lib/server/` or use `'use server'`
- Never import server code in client components

## Component Patterns
- Use Server Components by default
- Add `'use client'` only when needed (interactivity, hooks, browser APIs)
- Use Server Actions for mutations
- Use React Server Components for data fetching
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

Next.js includes TypeScript support out of the box. The `tsconfig.json` is pre-configured.

### Type Definitions

Next.js provides excellent type inference:

```typescript
// app/src/app/dashboard/page.tsx
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'Dashboard',
};

export default async function DashboardPage() {
  // Server Component - can use async/await
  const data = await getDashboardData();
  return <div>{/* ... */}</div>;
}
```

### App Types

Define global types:

```typescript
// app/src/types/index.ts
export interface User {
  id: string;
  name: string;
  email: string;
}

// app/src/app/layout.tsx
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'My App',
  description: 'App description',
};
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

Creates optimized production build in `.next/`.

### Preview Production Build

```bash
npm run start
```

### Running Tests

```bash
# If using Jest
npm run test

# If using Vitest
npm run test
```

### Linting

```bash
npm run lint
```

### Type Checking

```bash
npm run type-check
# or
npx tsc --noEmit
```

---

## Best Practices

### 1. Component Organization

- **Group by feature:** `components/users/UserCard.tsx`
- **Group by type:** `components/ui/Button.tsx`
- **Use index files:** `components/ui/index.ts` for easier imports

### 2. Server vs Client Components

- **Server Components by default** - No `'use client'` directive
- **Add `'use client'`** only when needed:
  - Using React hooks (useState, useEffect, etc.)
  - Using browser APIs (window, document, etc.)
  - Event handlers (onClick, onChange, etc.)
  - Third-party libraries that require client-side

```typescript
// Server Component (default)
// app/src/app/dashboard/page.tsx
export default async function DashboardPage() {
  const data = await getData(); // Can use async/await
  return <div>{data}</div>;
}

// Client Component
// app/src/components/Counter.tsx
'use client';

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

### 3. Data Fetching

- **Server Components:** Use async/await directly
- **Client Components:** Use `useEffect` with fetch or React Query
- **API Routes:** Use `route.ts` files for REST APIs

```typescript
// Server Component - Direct data fetching
// app/src/app/users/page.tsx
export default async function UsersPage() {
  const users = await getUsers();
  return <UsersList users={users} />;
}

// API Route
// app/src/app/api/users/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  const users = await getUsers();
  return NextResponse.json(users);
}
```

### 4. Server Actions

- **Use Server Actions** for form submissions and mutations
- **Mark with `'use server'`** directive

```typescript
// app/src/lib/server/actions.ts
'use server';

export async function createUser(formData: FormData) {
  const name = formData.get('name');
  // Server-side logic
  await saveUser(name);
}
```

```typescript
// app/src/components/CreateUserForm.tsx
'use client';

import { createUser } from '@/lib/server/actions';

export function CreateUserForm() {
  return (
    <form action={createUser}>
      <input name="name" required />
      <button type="submit">Create</button>
    </form>
  );
}
```

### 5. Routing

- **File-based routing** in `app/` directory
- **Route groups** with `(groupName)` for organization
- **Dynamic routes** with `[param]` segments
- **Catch-all routes** with `[...slug]`

### 6. Error Handling

- **Use `error.tsx`** for error boundaries
- **Use `not-found.tsx`** for 404 pages
- **Handle errors in Server Components**

```typescript
// app/src/app/dashboard/error.tsx
'use client';

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

### 7. Performance

- **Use Server Components** to reduce client bundle size
- **Use `next/image`** for image optimization
- **Use `next/link`** for client-side navigation
- **Lazy load components** when appropriate
- **Use React Suspense** for loading states

---

## Common Patterns

### Pattern 1: Server Component with Data Fetching

**Use case:** Loading data on the server

**Example:**
```typescript
// app/src/app/users/page.tsx
import { getUsers } from '@/lib/server/db';

export default async function UsersPage() {
  const users = await getUsers();
  
  return (
    <div>
      <h1>Users</h1>
      <UsersList users={users} />
    </div>
  );
}
```

**LLM Agent Notes:**
- Clear pattern: Server Components can use async/await
- No `'use client'` directive needed
- Type-safe with TypeScript

### Pattern 2: API Route

**Use case:** Creating REST API endpoints

**Example:**
```typescript
// app/src/app/api/users/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { getUsers, createUser } from '@/lib/server/db';

export async function GET() {
  const users = await getUsers();
  return NextResponse.json(users);
}

export async function POST(request: NextRequest) {
  const body = await request.json();
  const user = await createUser(body);
  return NextResponse.json(user, { status: 201 });
}
```

**LLM Agent Notes:**
- Standard HTTP method handlers
- Type-safe with Next.js types
- Clear error handling pattern

### Pattern 3: Server Action

**Use case:** Form submissions and mutations

**Example:**
```typescript
// app/src/lib/server/actions.ts
'use server';

import { revalidatePath } from 'next/cache';
import { createUser } from '@/lib/server/db';

export async function createUserAction(formData: FormData) {
  const name = formData.get('name') as string;
  
  if (!name) {
    return { error: 'Name is required' };
  }
  
  await createUser(name);
  revalidatePath('/users');
  return { success: true };
}
```

```typescript
// app/src/components/CreateUserForm.tsx
'use client';

import { createUserAction } from '@/lib/server/actions';
import { useFormState } from 'react-dom';

export function CreateUserForm() {
  const [state, formAction] = useFormState(createUserAction, null);
  
  return (
    <form action={formAction}>
      <input name="name" required />
      {state?.error && <p>{state.error}</p>}
      <button type="submit">Create User</button>
    </form>
  );
}
```

**LLM Agent Notes:**
- Clear separation: action in server file, form in client component
- Type-safe with TypeScript
- Progressive enhancement with Server Actions

### Pattern 4: Client Component with Interactivity

**Use case:** Components needing interactivity

**Example:**
```typescript
// app/src/components/Counter.tsx
'use client';

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**LLM Agent Notes:**
- Must use `'use client'` directive
- Standard React patterns
- Clear when client-side is needed

---

## Troubleshooting

### Common Issues

**Issue 1: Server code imported in client component**
- **Error:** "You're importing a component that needs server-only"
- **Solution:** Ensure server code is in `lib/server/` or uses `'use server'`
- **Check:** Verify imports in client components don't include server-only code

**Issue 2: Hooks used in Server Component**
- **Error:** "useState only works in Client Components"
- **Solution:** Add `'use client'` directive to component
- **Check:** Ensure Server Components don't use React hooks

**Issue 3: Routes not working**
- **Error:** 404 on routes
- **Solution:** Check file naming (`page.tsx`, not `Page.tsx`)
- **Check:** Verify route structure matches file structure in `app/`

**Issue 4: API routes not working**
- **Error:** 404 on API routes
- **Solution:** Ensure route file is named `route.ts` in `app/api/[endpoint]/`
- **Check:** Verify HTTP method exports (GET, POST, etc.)

### Getting Help

- Next.js Documentation: https://nextjs.org/docs
- Next.js Discord: https://nextjs.org/discord
- Next.js GitHub: https://github.com/vercel/next.js

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
- [Next.js Documentation](https://nextjs.org/docs)

---

**Document Version:** 1.0  
**Last Updated:** 2025-01-XX  
**Next.js Version:** Latest (App Router)

