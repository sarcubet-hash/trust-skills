---
name: frontend-development
description: Guidelines for Vue 3 frontend development, including component rules, TypeScript typing, state management, and project structure. Use when writing frontend Vue code.
---

# Frontend Development Guidelines (Vue 3 + Vite)

## When to use this skill
- When creating or modifying Vue components.
- When organizing frontend project structure.
- When integrating APIs in the frontend.
- When configuring TypeScript types or state management in Vue.

## Workflow
- Apply Vue 3 Component conventions strictly.
- Enforce TypeScript formatting and avoid `any`.
- Restrict logic locations (SRP).

## Instructions

### Component & Naming Standards
- **Components:** Use `PascalCase` for all Vue components.
  - File names for components should also follow `PascalCase.vue`.
- **Variables and functions:** Use `camelCase`.
- **Composables:** Prefix with `use` (e.g., `useAuth`, `useApi`).
- **Constants:** Use `UPPER_SNAKE_CASE`.

### Typing & Type Safety
- **Strict TypeScript:** Use strict TypeScript configuration.
- **Explicit Definitions:** Define explicit types for:
  - All `props`
  - `Emits`
  - Reactive state
  - API responses
- **No `any`:** Avoid using `any`. If unavoidable, document the reason.
- **Reusable Types:** Create reusable types/interfaces in a dedicated `types` folder.
- **Services:** Strongly type API service layers.

### Project Structure (Vue 3 + Vite)
Suggested structure:
- `components/` – Reusable UI components
- `views/` – Page-level components
- `composables/` – Reusable logic using Composition API
- `services/` – API handling
- `stores/` – State management (Pinia if used)
- `types/` – Type definitions
- `utils/` – Utility functions

**Rule:** Keep separation of concerns clear. Avoid mixing API logic inside components.

### Component Best Practices
- **Composition API:** Use Composition API consistently.
- **SRP:** Keep components small and focused (Single Responsibility Principle).
- **Reusable Logic:** Extract reusable logic into composables.
- **Nesting:** Avoid deeply nested components where possible.
- **Props & Emits:** Use `defineProps` and `defineEmits` with proper typing.
- **Validation:** Validate props properly and define defaults where needed.
- **Imports:** Remove all direct class/module paths inside logic functions and always import them at the top of the file.

### State Management
- **Pinia:** Use Pinia for global state management (if applicable).
- **Local State First:** Avoid unnecessary global state — prefer local state when possible.
- **Mutation Rules:** Do not mutate state directly outside defined store actions.
- **Modularity:** Keep stores modular and feature-based.

### API & Data Handling
- **Centralize API:** Centralize API calls inside `services`.
- **No Direct Calls:** Do not call APIs directly inside templates.
- **UX States:** Handle loading, error, and empty states explicitly.

### Performance Best Practices
- **Lazy Loading:** Use `defineAsyncComponent` for lazy loading where needed.
- **Routes:** Lazy load routes.
- **Watchers:** Avoid unnecessary watchers. Use `computed` over `watch` when possible.
- **Keys:** Add proper `key` attributes in loops.
- **Reactivity Check:** Avoid large reactive objects when not necessary.

### Code Quality
- **ESLint:** Follow consistent ESLint configuration.
- **Cleanup:** Remove unused imports and variables.
- **No Magic:** Avoid magic strings — use constants or enums.
- **Reusability:** Write reusable UI components instead of duplicating markup.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
