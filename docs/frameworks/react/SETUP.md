# React Setup Guide

**Framework:** React  
**Type:** Frontend-only  
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

React is a popular JavaScript library for building user interfaces. It's particularly well-suited for LLM agent collaboration due to its extensive training data in LLM models, clear component patterns, and large ecosystem. React is typically used for Single Page Applications (SPAs) and requires a separate backend API.

### Key Features

- Component-based architecture
- Virtual DOM for performance
- Large ecosystem and community
- Extensive third-party libraries
- Strong TypeScript support
- Flexible and unopinionated

### LLM Agent Compatibility

**Rating:** ⭐⭐⭐⭐⭐ Excellent

**Why:**
- React is extensively trained in LLM models
- Abundant examples and patterns in training data
- Clear component structure
- Strong TypeScript support
- Excellent documentation
- Largest ecosystem of any frontend framework

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

**Option A: Vite (Recommended)**

```bash
npm create vite@latest my-app -- --template react-ts
```

**Option B: Create React App (Legacy)**

```bash
npx create-react-app my-app --template typescript
```

**Recommendation:** Use Vite for faster development experience and better performance.

### Step 2: Install Dependencies

```bash
cd my-app
npm install
```

### Step 3: Install Additional Dependencies (Optional)

```bash
# Routing
npm install react-router-dom

# State management (if needed)
npm install zustand
# or
npm install @reduxjs/toolkit react-redux

# HTTP client
npm install axios
```

### Step 4: Verify Setup

```bash
npm run dev
```

Visit `http://localhost:5173` (Vite) or `http://localhost:3000` (CRA) to verify the application is running.

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
├── frontend/                      # React application
│   ├── src/
│   │   ├── components/            # React components
│   │   │   ├── ui/                # UI components
│   │   │   ├── forms/             # Form components
│   │   │   └── layout/            # Layout components
│   │   ├── pages/                 # Page components
│   │   ├── hooks/                 # Custom React hooks
│   │   ├── lib/                   # Utilities and helpers
│   │   │   ├── api/               # API client
│   │   │   └── utils/             # Utility functions
│   │   ├── store/                 # State management (if using)
│   │   ├── types/                 # TypeScript types
│   │   ├── styles/                # Global styles
│   │   ├── App.tsx                # Root component
│   │   └── main.tsx               # Entry point
│   ├── public/                    # Static assets
│   ├── package.json
│   ├── tsconfig.json              # TypeScript config
│   ├── vite.config.ts             # Vite configuration (if using Vite)
│   └── README.md
├── backend/                       # Backend API (separate)
│   └── ...
└── README.md                      # Project overview
```

### Key Directories

**`frontend/src/components/`** - Reusable React components
- Organize by feature or type (e.g., `ui/`, `forms/`, `layout/`)
- Use PascalCase for component files: `Button.tsx`, `UserCard.tsx`

**`frontend/src/pages/`** - Page components
- Top-level route components
- Each page corresponds to a route

**`frontend/src/hooks/`** - Custom React hooks
- Reusable hook logic
- Use `use` prefix: `useAuth.ts`, `useApi.ts`

**`frontend/src/lib/api/`** - API client
- Functions for API calls
- Centralized API configuration

**`frontend/src/store/`** - State management
- Zustand stores, Redux slices, or Context providers
- Global application state

**`frontend/src/types/`** - TypeScript types
- Shared type definitions
- API response types

**`frontend/public/`** - Static assets
- Images, fonts, favicon
- Files served as-is

### React-Specific Structure

```
frontend/src/
├── components/
│   ├── ui/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── index.ts              # Barrel exports
│   ├── forms/
│   │   └── LoginForm.tsx
│   └── layout/
│       ├── Header.tsx
│       ├── Footer.tsx
│       └── Layout.tsx
├── pages/
│   ├── Home.tsx
│   ├── About.tsx
│   └── Dashboard.tsx
├── hooks/
│   ├── useAuth.ts
│   ├── useApi.ts
│   └── useLocalStorage.ts
├── lib/
│   ├── api/
│   │   ├── client.ts             # API client setup
│   │   ├── users.ts              # User API calls
│   │   └── auth.ts               # Auth API calls
│   └── utils/
│       ├── format.ts
│       └── validation.ts
├── store/
│   ├── authStore.ts              # Zustand store example
│   └── userStore.ts
├── types/
│   ├── user.ts
│   └── api.ts
├── styles/
│   ├── globals.css
│   └── variables.css
├── App.tsx
└── main.tsx
```

---

## LLM Agent Configuration

### .cursorrules Example

```markdown
# Project Rules for LLM Agents

## Framework: React (SPA)

### Project Structure
- Follow React best practices
- Components in `frontend/src/components/`
- Pages in `frontend/src/pages/`
- Hooks in `frontend/src/hooks/`
- API client in `frontend/src/lib/api/`
- See: docs/specs/NAMING_CONVENTIONS.md
- See: docs/specs/DEVELOPMENT_SETUP.md

## Code Standards
- Use TypeScript for all new code
- Follow React best practices and hooks rules
- Component naming: PascalCase (e.g., `UserCard.tsx`)
- File naming: PascalCase for components, camelCase for utilities
- Use functional components (not class components)
- Use React hooks for state and side effects

## React-Specific Rules
- Use functional components exclusively
- Use hooks for state management (useState, useEffect, etc.)
- Use custom hooks for reusable logic
- Keep components small and focused
- Extract logic to custom hooks when needed
- Use TypeScript for props and state

## Component Patterns
- Use props for component communication
- Use Context for deeply nested prop passing (sparingly)
- Use state management library (Zustand/Redux) for global state
- Prefer composition over inheritance
- Use React.memo for performance optimization when needed

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

React with TypeScript requires proper type definitions:

```typescript
// frontend/src/vite-env.d.ts (Vite)
/// <reference types="vite/client" />

// frontend/src/react-app-env.d.ts (Create React App)
/// <reference types="react-scripts" />
```

### Component Types

```typescript
// frontend/src/components/Button.tsx
import { ReactNode } from 'react';

interface ButtonProps {
  children: ReactNode;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

export function Button({ children, onClick, variant = 'primary', disabled }: ButtonProps) {
  return (
    <button onClick={onClick} className={`btn btn-${variant}`} disabled={disabled}>
      {children}
    </button>
  );
}
```

### Hook Types

```typescript
// frontend/src/hooks/useAuth.ts
import { useState, useEffect } from 'react';
import type { User } from '@/types/user';

export function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Auth logic
  }, []);

  return { user, loading };
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

Creates optimized production build in `dist/` (Vite) or `build/` (CRA).

### Preview Production Build

```bash
# Vite
npm run preview

# Create React App
npm run start
```

### Running Tests

```bash
npm test
```

### Linting

```bash
npm run lint
```

### Type Checking

```bash
# Vite
npm run type-check
# or
npx tsc --noEmit

# Create React App
npm run type-check
```

---

## Best Practices

### 1. Component Organization

- **Group by feature:** `components/users/UserCard.tsx`
- **Group by type:** `components/ui/Button.tsx`
- **Use index files:** `components/ui/index.ts` for easier imports
- **Keep components small:** Single responsibility principle

### 2. State Management

- **Local state:** Use `useState` for component-specific state
- **Shared state:** Use Context API for simple shared state
- **Global state:** Use Zustand or Redux for complex global state
- **Server state:** Use React Query or SWR for server data

```typescript
// Local state
const [count, setCount] = useState(0);

// Context for shared state
const AuthContext = createContext<AuthContextType | null>(null);

// Zustand for global state
import { create } from 'zustand';

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
}

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}));
```

### 3. Custom Hooks

- **Extract reusable logic** into custom hooks
- **Use `use` prefix** for hook names
- **Return objects** for multiple values

```typescript
// frontend/src/hooks/useApi.ts
import { useState, useEffect } from 'react';

export function useApi<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((err) => {
        setError(err);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}
```

### 4. API Integration

- **Centralize API calls** in `lib/api/`
- **Use typed API client** (axios with TypeScript)
- **Handle errors consistently**

```typescript
// frontend/src/lib/api/client.ts
import axios from 'axios';

const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

export default apiClient;

// frontend/src/lib/api/users.ts
import apiClient from './client';
import type { User } from '@/types/user';

export async function getUsers(): Promise<User[]> {
  const { data } = await apiClient.get<User[]>('/users');
  return data;
}
```

### 5. Routing

- **Use React Router** for navigation
- **Define routes** in a central location
- **Use typed routes** with TypeScript

```typescript
// frontend/src/App.tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Home } from './pages/Home';
import { About } from './pages/About';
import { Dashboard } from './pages/Dashboard';

export function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### 6. Error Handling

- **Use Error Boundaries** for component error handling
- **Handle API errors** consistently
- **Provide user-friendly error messages**

```typescript
// frontend/src/components/ErrorBoundary.tsx
import { Component, ReactNode } from 'react';

interface Props {
  children: ReactNode;
}

interface State {
  hasError: boolean;
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

### 7. Performance

- **Use React.memo** for expensive components
- **Use useMemo and useCallback** to prevent unnecessary re-renders
- **Code splitting** with React.lazy
- **Optimize images** and assets

```typescript
// Code splitting
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./pages/Dashboard'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  );
}
```

---

## Common Patterns

### Pattern 1: Functional Component with Hooks

**Use case:** Standard React component

**Example:**
```typescript
// frontend/src/components/UserCard.tsx
import { useState } from 'react';
import type { User } from '@/types/user';

interface UserCardProps {
  user: User;
}

export function UserCard({ user }: UserCardProps) {
  const [expanded, setExpanded] = useState(false);

  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <button onClick={() => setExpanded(!expanded)}>
        {expanded ? 'Collapse' : 'Expand'}
      </button>
      {expanded && <p>{user.email}</p>}
    </div>
  );
}
```

**LLM Agent Notes:**
- Clear functional component pattern
- Type-safe props with TypeScript
- Standard React hooks usage

### Pattern 2: Custom Hook

**Use case:** Reusable logic

**Example:**
```typescript
// frontend/src/hooks/useLocalStorage.ts
import { useState, useEffect } from 'react';

export function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue] as const;
}
```

**LLM Agent Notes:**
- Clear hook pattern with TypeScript generics
- Reusable across components
- Type-safe return values

### Pattern 3: API Integration

**Use case:** Fetching data from backend

**Example:**
```typescript
// frontend/src/hooks/useUsers.ts
import { useState, useEffect } from 'react';
import { getUsers } from '@/lib/api/users';
import type { User } from '@/types/user';

export function useUsers() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    getUsers()
      .then(setUsers)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);

  return { users, loading, error };
}
```

**LLM Agent Notes:**
- Clear data fetching pattern
- Type-safe API calls
- Consistent error handling

---

## Troubleshooting

### Common Issues

**Issue 1: Hooks called conditionally**
- **Error:** "React Hook useEffect is called conditionally"
- **Solution:** Always call hooks at the top level of components
- **Check:** Ensure hooks are not inside if statements or loops

**Issue 2: Missing dependencies in useEffect**
- **Error:** ESLint warning about missing dependencies
- **Solution:** Add all dependencies to dependency array or use eslint-disable comment with explanation
- **Check:** Review useEffect dependency arrays

**Issue 3: Type errors with props**
- **Error:** Type errors when using components
- **Solution:** Ensure props interface matches usage
- **Check:** Verify TypeScript types are correctly defined

### Getting Help

- React Documentation: https://react.dev
- React TypeScript Cheatsheet: https://react-typescript-cheatsheet.netlify.app
- React GitHub: https://github.com/facebook/react

---

## Next Steps

1. Review [Framework Selection Guide](../FRAMEWORK_SELECTION.md) if you haven't already
2. Set up project structure following this guide
3. Configure `.cursorrules` for LLM agent collaboration
4. Set up backend API (separate service)
5. Create initial documentation in `docs/specs/`
6. Follow [LLM Agent Project Setup Guide](../../LLM_AGENT_PROJECT_SETUP.md) for complete project setup

---

## Related Documentation

- [Framework Selection Guide](../FRAMEWORK_SELECTION.md)
- [LLM Agent Project Setup Guide](../../LLM_AGENT_PROJECT_SETUP.md)
- [Project Structure](../../LLM_AGENT_PROJECT_SETUP.md#project-structure)
- [LLM Agent Best Practices](../../LLM_AGENT_PROJECT_SETUP.md#llm-agent-best-practices)
- [React Documentation](https://react.dev)

---

**Document Version:** 1.0  
**Last Updated:** 2025-01-XX  
**React Version:** Latest

