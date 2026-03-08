---
name: backend-architecture
description: Guidelines for backend architecture, clean code principles, testing requirements, and the New API Development Flow. Use when creating new APIs, making architectural decisions, or writing backend code.
---

# Backend Architecture Guidelines

## When to use this skill
- When creating a new API or backend endpoint.
- When deciding how to structure and layer backend business logic.
- When writing controllers, services, repositories, or models.
- When reviewing backend code for architectural compliance.

## Workflow: New API Development Flow
When creating a new API, strictly follow this structure and workflow in order:
1. Create endpoint in `api.php`
2. Create controller in `Controllers`
3. Create request class in `Requests`
4. Create service in `Services`
5. Create repository interface
6. Create repository in `Repositories`
7. Create model in `Models` (if required)
8. Create migration in `Migrations` (if required)
9. Add presenter in `Presenters`
10. Add unit tests in `tests/Unit`
11. Add feature tests in `tests/Feature`
12. Update API documentation in `openapi.json`

## Instructions

### Architecture Guidelines
- **Pattern:** Strictly use the **Service Repository pattern**.
- **Reference:** When creating a new API, use an existing API as a reference.
- **Consistency:** Even if some older APIs don't follow the Service Repository pattern, **always follow it for new implementations**.

### Code Standards
- **Imports:** Remove all direct class paths in the code (e.g., `\Illuminate\Container\Container`) and always import them at the top of the file.
- **Coding Style:** Follow PSR-12 coding standards strictly.
- **Type Hints:** Always use type hints. They are required for **all** arguments and return types across:
  - Controllers
  - Services
  - Repositories
  - Models
  - Requests
  - Presenters
  - Any other classes
- **Error Handling:** Controllers must include try-catch blocks and return proper error messages along with `$e->getMessage()` and `$e->getTraceAsString()` to easily trace the error context.

### Clean Code Principles
- **No Magic:** Prefer Enums/Constants over magic strings or magic numbers.
- **Naming Conventions:**
  - `PascalCase` for classes.
  - `camelCase` for variables.
- **Performance:** Strictly avoid N+1 queries. Ensure efficient eager loading where necessary.

### Testing Requirements
- Ensure full feature coverage with both Unit and Feature tests.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
