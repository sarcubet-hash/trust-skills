---
name: frontend-development
description: Guidelines for Vue 3 frontend development, including component rules, TypeScript typing, state management, and project structure. Use when the user says any of: create component, add component, make component, build component, write vue, add vue file, create page, add page, build frontend, write frontend, add composable, create composable, update store, add store, create service (frontend), add type, create type, write TypeScript, fix vue, refactor component, follow frontend standards, or any request to generate Vue/TypeScript/frontend code.
---

# Frontend Development Guidelines (Vue 3 + Vite)

## When to use this skill
- When the user says: **create**, **add**, **build**, **make**, **implement**, **write**, **generate**, **update**, or **refactor** — and the context involves frontend code.
- Keyword triggers: `component`, `vue`, `page`, `view`, `composable`, `store`, `pinia`, `typescript`, `ts`, `frontend`, `ui`, `template`, `props`, `emit`, `ref`, `reactive`, `computed`.
- When creating or modifying Vue components.
- When organizing frontend project structure.
- When integrating APIs in the frontend.
- When configuring TypeScript types or state management in Vue.
- When the user asks to follow, refer, or use frontend standards.

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
- **Lint Fix — Staged Files Only:**
  - When running ESLint fixes, **only target the files that are currently staged**, never auto-fix the entire project.
  - Resolve staged file list first (run automatically, `SafeToAutoRun: true`):
    ```bash
    git diff --name-only --cached
    ```
  - Then run ESLint only on those files:
    ```bash
    npx eslint --fix <file1> <file2> ...
    ```
  - **Never run** `npx eslint --fix .` or any command that fixes all files in the project — this can introduce unintended changes to files the user hasn't touched.
- **Cleanup:** Remove unused imports and variables.
- **No Magic:** Avoid magic strings — use constants or enums.
- **Reusability:** Write reusable UI components instead of duplicating markup.

### Modern Frontend Features
**Always try to use the latest Vue / TypeScript / Vite features, but verify version compatibility first.**

#### Step 1 — Detect versions (run automatically, `SafeToAutoRun: true`)
```bash
# Node version
node -v
# Vue and TypeScript versions from project
cat package.json | grep -E '"vue"|"typescript"|"vite"'
```

#### Step 2 — Apply features based on detected versions

**Vue 3 — feature by version:**

| Feature | Min Vue Version | Example / Notes |
|---|---|---|
| `<script setup>` | 3.2 | Replaces `setup()` boilerplate |
| `v-bind` in `<style>` | 3.2 | Bind reactive vars to CSS |
| `defineSlots` | 3.3 | Type-safe slot declarations |
| Generic components | 3.3 | `<script setup lang="ts" generic="T">` |
| `defineModel()` | 3.4 | Two-way binding shorthand (replaces `modelValue` pattern) |
| `v-bind` shorthand `:prop` same-name | 3.4 | `:id` instead of `:id="id"` |
| `useTemplateRef()` | 3.5 | Replaces `ref()` for template refs |
| Deferred Teleport | 3.5 | `<Teleport defer>` |
| `onWatcherCleanup()` | 3.5 | Cleaner watcher teardown |
| `watch` with deep option on reactive | 3.5 | Granular deep watching |

**TypeScript — feature by version:**

| Feature | Min TS Version | Notes |
|---|---|---|
| Template literal types | 4.1 | `` `${Status}Event` `` |
| `satisfies` operator | 4.9 | Type check without widening |
| `const` type parameters | 5.0 | Infers literal types in generics |
| Variadic tuple types | 4.0 | `[...T, ...U]` |
| `using` / `await using` (Explicit Resource Management) | 5.2 | Auto-cleanup |

> **📌 Note:** The tables above are a baseline reference. As new Vue and TypeScript versions are released, **always use your knowledge of those newer versions too**. If the detected version is higher than the latest one listed in these tables, apply the latest available modern features from that version. The goal is to always write code using the **most up-to-date stable features** the project supports — not to be limited to what's listed here.

- **Only use a feature if the detected version supports it.**
- **Prefer modern patterns**: `defineModel` over manual `modelValue/emit`, `useTemplateRef` over raw `ref()` for DOM access, `satisfies` over type assertions, `<script setup>` always.
- **Stay current:** If a newer Vue or TypeScript version is detected that isn't in the tables, use your knowledge of that version's features and apply them where appropriate.

#### Step 3 — Announce modern features used
After generating the code, **always** add a brief note in the following format at the very end of your response:

> **Format rule:** Use an HTML `<span>` with `style="color: #16a34a; font-weight: bold"` for the title and `style="color: #16a34a"` for the body. Keep the language casual and human-friendly — explain *what* was used and *why it's better*, in 2-4 sentences max.

```html
<span style="color: #16a34a; font-weight: bold">✅ Modern/Latest Features Added from Vue {X.Y} / TypeScript {X.Y}</span>
<span style="color: #16a34a">
Brief, friendly explanation of what modern features were used in the code above and why.
Example: "Used defineModel() (Vue 3.4) instead of the old modelValue + emit pattern — way less boilerplate.
Also used useTemplateRef() to grab DOM elements more cleanly."
</span>
```

- Replace `{X.Y}` with the actual detected versions.
- If no modern features were applicable in the generated code, **skip this block entirely**.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
