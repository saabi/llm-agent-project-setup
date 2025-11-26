# Vue.js Setup Guide

**Framework:** Vue.js  
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

Vue.js is a progressive JavaScript framework for building user interfaces. It's known for its gentle learning curve, excellent documentation, and flexible architecture. Vue.js is particularly well-suited for LLM agent collaboration due to its clear patterns, strong TypeScript support, and predictable structure.

### Key Features

- Progressive framework (can be adopted incrementally)
- Component-based architecture
- Reactive data binding
- Template-based syntax (HTML-like)
- Strong TypeScript support
- Excellent documentation

### LLM Agent Compatibility

**Rating:** ⭐⭐⭐⭐ Good

**Why:**
- Clear, predictable patterns
- Good documentation
- Strong TypeScript support
- Template syntax is easy to understand
- Moderate training data in LLM models
- Consistent component structure

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
npm create vue@latest my-app
```

**Options to select:**
- **TypeScript:** Yes (highly recommended for LLM agents)
- **JSX:** No (use template syntax)
- **Router:** Yes (recommended)
- **Pinia:** Yes (recommended for state management)
- **Vitest:** Optional (for testing)
- **ESLint:** Yes (recommended)

**Option B: Vue CLI (Legacy)**

```bash
npm install -g @vue/cli
vue create my-app
```

**Recommendation:** Use Vite for faster development experience.

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
├── frontend/                      # Vue.js application
│   ├── src/
│   │   ├── components/            # Vue components
│   │   │   ├── ui/                # UI components
│   │   │   ├── forms/             # Form components
│   │   │   └── layout/            # Layout components
│   │   ├── views/                 # Page components
│   │   ├── composables/           # Composition API functions
│   │   ├── stores/                # Pinia stores (state management)
│   │   ├── router/                # Vue Router configuration
│   │   ├── lib/                   # Utilities and helpers
│   │   │   ├── api/               # API client
│   │   │   └── utils/             # Utility functions
│   │   ├── types/                 # TypeScript types
│   │   ├── assets/                # Assets (images, styles)
│   │   ├── App.vue                # Root component
│   │   └── main.ts                # Entry point
│   ├── public/                    # Static assets
│   ├── package.json
│   ├── tsconfig.json              # TypeScript config
│   ├── vite.config.ts             # Vite configuration
│   └── README.md
├── backend/                       # Backend API (separate)
│   └── ...
└── README.md                      # Project overview
```

### Key Directories

**`frontend/src/components/`** - Reusable Vue components
- Organize by feature or type (e.g., `ui/`, `forms/`, `layout/`)
- Use PascalCase for component files: `Button.vue`, `UserCard.vue`

**`frontend/src/views/`** - Page components
- Top-level route components
- Each view corresponds to a route

**`frontend/src/composables/`** - Composition API functions
- Reusable composition functions
- Use `use` prefix: `useAuth.ts`, `useApi.ts`

**`frontend/src/stores/`** - Pinia stores
- Global state management
- Organized by feature

**`frontend/src/router/`** - Vue Router configuration
- Route definitions
- Navigation guards

**`frontend/src/lib/api/`** - API client
- Functions for API calls
- Centralized API configuration

**`frontend/src/types/`** - TypeScript types
- Shared type definitions
- API response types

**`frontend/public/`** - Static assets
- Images, fonts, favicon
- Files served as-is

### Vue.js-Specific Structure

```
frontend/src/
├── components/
│   ├── ui/
│   │   ├── Button.vue
│   │   ├── Input.vue
│   │   └── index.ts              # Barrel exports
│   ├── forms/
│   │   └── LoginForm.vue
│   └── layout/
│       ├── Header.vue
│       ├── Footer.vue
│       └── Layout.vue
├── views/
│   ├── HomeView.vue
│   ├── AboutView.vue
│   └── DashboardView.vue
├── composables/
│   ├── useAuth.ts
│   ├── useApi.ts
│   └── useLocalStorage.ts
├── stores/
│   ├── auth.ts                   # Pinia store
│   └── user.ts
├── router/
│   └── index.ts                  # Router configuration
├── lib/
│   ├── api/
│   │   ├── client.ts             # API client setup
│   │   ├── users.ts              # User API calls
│   │   └── auth.ts               # Auth API calls
│   └── utils/
│       ├── format.ts
│       └── validation.ts
├── types/
│   ├── user.ts
│   └── api.ts
├── assets/
│   ├── styles/
│   │   └── main.css
│   └── images/
├── App.vue
└── main.ts
```

---

## LLM Agent Configuration

### .cursorrules Example

```markdown
# Project Rules for LLM Agents

## Framework: Vue.js (SPA)

### Project Structure
- Follow Vue.js best practices
- Components in `frontend/src/components/`
- Views in `frontend/src/views/`
- Composables in `frontend/src/composables/`
- Stores in `frontend/src/stores/`
- See: docs/specs/NAMING_CONVENTIONS.md
- See: docs/specs/DEVELOPMENT_SETUP.md

## Code Standards
- Use TypeScript for all new code
- Use Composition API (not Options API)
- Component naming: PascalCase (e.g., `UserCard.vue`)
- File naming: PascalCase for components, camelCase for utilities
- Use `<script setup>` syntax for components
- Use TypeScript for props and emits

## Vue.js-Specific Rules
- Use Composition API with `<script setup>`
- Use `defineProps` and `defineEmits` for component props/emits
- Use composables for reusable logic
- Use Pinia for state management
- Use Vue Router for routing
- Keep components small and focused

## Component Patterns
- Use props for component communication
- Use emits for child-to-parent communication
- Use provide/inject for deeply nested communication (sparingly)
- Use Pinia stores for global state
- Prefer composition over inheritance

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

Vue.js with TypeScript requires proper type definitions:

```typescript
// frontend/src/env.d.ts
/// <reference types="vite/client" />

declare module '*.vue' {
  import type { DefineComponent } from 'vue';
  const component: DefineComponent<{}, {}, any>;
  export default component;
}
```

### Component Types

```typescript
// frontend/src/components/Button.vue
<script setup lang="ts">
interface Props {
  label: string;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

interface Emits {
  (e: 'click'): void;
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'primary',
  disabled: false,
});

const emit = defineEmits<Emits>();

const handleClick = () => {
  if (!props.disabled) {
    emit('click');
  }
};
</script>

<template>
  <button
    :class="`btn btn-${variant}`"
    :disabled="disabled"
    @click="handleClick"
  >
    {{ label }}
  </button>
</template>
```

### Composable Types

```typescript
// frontend/src/composables/useAuth.ts
import { ref, computed } from 'vue';
import type { User } from '@/types/user';

export function useAuth() {
  const user = ref<User | null>(null);
  const loading = ref(true);

  const isAuthenticated = computed(() => user.value !== null);

  const login = async (email: string, password: string) => {
    // Auth logic
  };

  return {
    user,
    loading,
    isAuthenticated,
    login,
  };
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

Creates optimized production build in `dist/`.

### Preview Production Build

```bash
npm run preview
```

### Running Tests

```bash
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
npx vue-tsc --noEmit
```

---

## Best Practices

### 1. Component Organization

- **Group by feature:** `components/users/UserCard.vue`
- **Group by type:** `components/ui/Button.vue`
- **Use index files:** `components/ui/index.ts` for easier imports
- **Keep components small:** Single responsibility principle

### 2. Composition API

- **Use `<script setup>`** syntax (recommended)
- **Use composables** for reusable logic
- **Use `ref` and `reactive`** appropriately

```typescript
// Use ref for primitives
const count = ref(0);

// Use reactive for objects
const state = reactive({
  user: null,
  loading: false,
});

// Use computed for derived state
const doubleCount = computed(() => count.value * 2);
```

### 3. State Management

- **Local state:** Use `ref` or `reactive` for component state
- **Global state:** Use Pinia stores
- **Server state:** Use composables with API calls

```typescript
// frontend/src/stores/user.ts
import { defineStore } from 'pinia';
import type { User } from '@/types/user';

export const useUserStore = defineStore('user', {
  state: (): { user: User | null } => ({
    user: null,
  }),
  getters: {
    isAuthenticated: (state) => state.user !== null,
  },
  actions: {
    setUser(user: User) {
      this.user = user;
    },
    clearUser() {
      this.user = null;
    },
  },
});
```

### 4. Composables

- **Extract reusable logic** into composables
- **Use `use` prefix** for composable names
- **Return reactive refs** for state

```typescript
// frontend/src/composables/useApi.ts
import { ref } from 'vue';

export function useApi<T>(url: string) {
  const data = ref<T | null>(null);
  const loading = ref(true);
  const error = ref<Error | null>(null);

  fetch(url)
    .then((res) => res.json())
    .then((result) => {
      data.value = result;
      loading.value = false;
    })
    .catch((err) => {
      error.value = err;
      loading.value = false;
    });

  return { data, loading, error };
}
```

### 5. Routing

- **Use Vue Router** for navigation
- **Define routes** in `router/index.ts`
- **Use typed routes** with TypeScript

```typescript
// frontend/src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router';
import HomeView from '@/views/HomeView.vue';

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView,
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('@/views/AboutView.vue'),
    },
  ],
});

export default router;
```

### 6. API Integration

- **Centralize API calls** in `lib/api/`
- **Use typed API client**
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

### 7. Performance

- **Use `v-memo`** for expensive lists
- **Use `v-once`** for static content
- **Lazy load components** with `defineAsyncComponent`
- **Optimize images** and assets

```typescript
// Lazy loading
import { defineAsyncComponent } from 'vue';

const AsyncComponent = defineAsyncComponent(() =>
  import('./components/HeavyComponent.vue')
);
```

---

## Common Patterns

### Pattern 1: Component with Composition API

**Use case:** Standard Vue component

**Example:**
```vue
<!-- frontend/src/components/UserCard.vue -->
<script setup lang="ts">
import { ref } from 'vue';
import type { User } from '@/types/user';

interface Props {
  user: User;
}

const props = defineProps<Props>();
const expanded = ref(false);
</script>

<template>
  <div class="user-card">
    <h3>{{ user.name }}</h3>
    <button @click="expanded = !expanded">
      {{ expanded ? 'Collapse' : 'Expand' }}
    </button>
    <p v-if="expanded">{{ user.email }}</p>
  </div>
</template>
```

**LLM Agent Notes:**
- Clear Composition API pattern
- Type-safe props with TypeScript
- Standard Vue template syntax

### Pattern 2: Composable

**Use case:** Reusable logic

**Example:**
```typescript
// frontend/src/composables/useLocalStorage.ts
import { ref, watch } from 'vue';

export function useLocalStorage<T>(key: string, initialValue: T) {
  const storedValue = ref<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  watch(storedValue, (newValue) => {
    try {
      window.localStorage.setItem(key, JSON.stringify(newValue));
    } catch (error) {
      console.error(error);
    }
  });

  return storedValue;
}
```

**LLM Agent Notes:**
- Clear composable pattern with TypeScript generics
- Reusable across components
- Type-safe return values

### Pattern 3: Pinia Store

**Use case:** Global state management

**Example:**
```typescript
// frontend/src/stores/auth.ts
import { defineStore } from 'pinia';
import type { User } from '@/types/user';

export const useAuthStore = defineStore('auth', {
  state: (): { user: User | null; token: string | null } => ({
    user: null,
    token: null,
  }),
  getters: {
    isAuthenticated: (state) => state.user !== null,
  },
  actions: {
    async login(email: string, password: string) {
      // Login logic
      this.user = user;
      this.token = token;
    },
    logout() {
      this.user = null;
      this.token = null;
    },
  },
});
```

**LLM Agent Notes:**
- Clear Pinia store pattern
- Type-safe state and actions
- Consistent state management

---

## Troubleshooting

### Common Issues

**Issue 1: Template refs not working**
- **Error:** "Cannot access ref before initialization"
- **Solution:** Ensure refs are defined before use in template
- **Check:** Verify ref names match between script and template

**Issue 2: Props type errors**
- **Error:** Type errors with props
- **Solution:** Ensure props interface matches usage
- **Check:** Verify TypeScript types are correctly defined with `defineProps`

**Issue 3: Reactive updates not working**
- **Error:** Changes not reflected in template
- **Solution:** Use `ref` or `reactive` for reactive state
- **Check:** Ensure you're accessing `.value` for refs

### Getting Help

- Vue.js Documentation: https://vuejs.org
- Vue.js TypeScript Guide: https://vuejs.org/guide/typescript/overview.html
- Vue.js GitHub: https://github.com/vuejs/core

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
- [Vue.js Documentation](https://vuejs.org)

---

**Document Version:** 1.0  
**Last Updated:** 2025-01-XX  
**Vue.js Version:** Latest (Composition API)

