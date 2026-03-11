---
name: backend-architecture
description: Guidelines for backend architecture, clean code principles, testing requirements, and the New API Development Flow. Use when the user says any of: create API, add endpoint, build route, add controller, write controller, make controller, add service, create service, write service, add repository, create repository, write repository, make model, create model, add migration, build backend, add backend logic, implement feature (backend), refactor backend, update controller, update service, update repository, follow backend standards, or any request to generate backend PHP/Laravel code.
---

# Backend Architecture Guidelines

## When to use this skill
- When the user says: **create**, **add**, **build**, **make**, **implement**, **write**, **generate**, **update**, or **refactor** — and the context involves backend code.
- Keyword triggers: `api`, `endpoint`, `route`, `controller`, `service`, `repository`, `model`, `migration`, `backend`, `php`, `laravel`, `artisan`.
- When creating a new API or backend endpoint.
- When deciding how to structure and layer backend business logic.
- When writing controllers, services, repositories, or models.
- When reviewing backend code for architectural compliance.
- When the user asks to follow, refer, or use backend standards.

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

### Modern PHP Features
**Always try to use the latest PHP features, but verify version compatibility first.**

#### Step 1 — Detect PHP version (run automatically, `SafeToAutoRun: true`)
```bash
php -v
```
If running in Docker: `bin/docker-dev exec app php -v`

#### Step 2 — Apply features based on the detected version
Use the table below to decide which modern features are safe to use:

| Feature | Min PHP Version | Example |
|---|---|---|
| Named arguments | 8.0 | `array_slice(array: $arr, offset: 1)` |
| Match expression | 8.0 | `match($status) { 1 => 'active' }` |
| Nullsafe operator | 8.0 | `$user?->profile?->bio` |
| Constructor property promotion | 8.0 | `public function __construct(private string $name)` |
| Union types | 8.0 | `int\|string $value` |
| Enums | 8.1 | `enum Status: string { case Active = 'active'; }` |
| Readonly properties | 8.1 | `public readonly string $name;` |
| First-class callables | 8.1 | `$fn = strlen(...)` |
| Fibers | 8.1 | `new Fiber(fn() => ...)` |
| Readonly classes | 8.2 | `readonly class DTO { ... }` |
| Disjunctive Normal Form types | 8.2 | `(Countable&Iterator)\|null` |
| Typed class constants | 8.3 | `const string VERSION = '1.0';` |
| `json_validate()` | 8.3 | `json_validate($str)` |
| Property hooks | 8.4 | `public string $name { get => ... set => ... }` |
| Asymmetric visibility | 8.4 | `public private(set) string $id;` |
| `array_find()` / `array_find_key()` | 8.4 | `array_find($arr, fn($v) => $v > 2)` |

> **📌 Note:** The table above is a baseline reference. As new PHP versions are released, **always use your knowledge of those newer versions too**. If the detected PHP version is higher than the latest version listed in the table, apply the latest available modern features from that version. The goal is to always write code using the **most up-to-date stable PHP features** the project supports — not to be limited to what's listed here.

- **Only use a feature if the detected PHP version supports it.**
- **Prefer modern features** (enums over class constants, readonly over manual immutability, match over switch, etc.) whenever they improve clarity.
- **Stay current:** If a newer PHP version is detected that isn't in the table, use your knowledge of that version's features and apply them where appropriate.

#### Step 3 — Announce modern features used
After generating the code, **always** add a brief note in the following format at the very end of your response:

> **Format rule:** Use an HTML `<span>` with `style="color: #16a34a; font-weight: bold"` for the title and `style="color: #16a34a"` for the body. Keep the language casual and human-friendly — explain *what* was used and *why it's better*, in 2-4 sentences max.

```html
<span style="color: #16a34a; font-weight: bold">✅ Modern/Latest Features Added from PHP {X.Y}</span>
<span style="color: #16a34a">
Brief, friendly explanation of what modern PHP features were used in the code above and why.
Example: "Used PHP 8.1 Enums instead of plain constants to make statuses type-safe and IDE-friendly.
Also used readonly properties so the DTO can't be accidentally mutated after construction."
</span>
```

- Replace `{X.Y}` with the actual detected PHP version (e.g., `PHP 8.3`).
- If no modern features were applicable in the generated code, **skip this block entirely**.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
