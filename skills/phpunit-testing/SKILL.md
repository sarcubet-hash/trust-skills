---
name: phpunit-testing
description: Guidelines for writing unit and feature tests using PHPUnit in Laravel projects. Use when the user asks to write tests or test guidelines.
---

# PHPUnit Testing Guidelines

## When to use this skill
- When requested to write tests or test guidelines using PHPUnit in a Laravel project.
- When generating unit or feature tests for classes, services, or controllers.

## Workflow
- Adhere strictly to the separation of Unit Tests and Feature Tests.
- Keep Unit Tests completely isolated, fast, and free of framework/DB dependencies.
- Keep Feature Tests focused on HTTP layer testing with the database state.
- Validate test files against the guidelines before accepting the implementation.

## Instructions

### Unit Test Guidelines
- Test one class in isolation.
- **Imports:** Remove all direct class paths in the code (e.g., `\Illuminate\Container\Container`) and always import them at the top of the file.
- No database interaction — no `RefreshDatabase`, no `assertDatabaseHas`/`assertDatabaseCount`, no Eloquent `create()`.
- No Laravel container — extend `PHPUnit\Framework\TestCase`, not `Tests\TestCase`.
- No HTTP layer or auth — no `actingAs()`, no `postJson()`, no `User::factory()->create()`.
- Mock all external dependencies — use `Mockery::mock()` for every injected service/model.
- Stub Eloquent model calls — use `Model::shouldReceive()` instead of hitting the DB.
- One file per class — e.g., `TrucareRepositoryTest.php`, `TrucareServiceTest.php`.
- Be fast and deterministic — no I/O, no network, no side effects.
- Use `/** @test */` annotation — no `test_` prefix on method names.
- No section-banner comments — method name is the only identity needed.

### Unit Test File & Folder Structure
Follow this folder structure exactly:
```
tests/Unit/
  Repositories/   → e.g., TrucareRepositoryTest.php
  Services/       → e.g., TrucareServiceTest.php
  Presenters/     → e.g., DocumentCategoryPresenterTest.php
```
- One test file covers **all methods** of the class it tests — do not split a single class across multiple files.
- Place repository tests under `tests/Unit/Repositories/`, service tests under `tests/Unit/Services/`, and presenter tests under `tests/Unit/Presenters/`.

### Unit Test Function Naming
Function names must be **snake_case**, fully descriptive, and readable as a sentence. They must describe the **scenario and expected outcome**. Follow this pattern:
```
<method_name>_<scenario>_<expected_result>
```
**Examples from existing tests:**
- `get_trucare_proposal_returns_flag_0_and_null_when_not_found`
- `print_and_sign_returns_flag_1_when_proposal_not_found`
- `print_proposal_list_view_returns_data_from_service`
- `build_payment_information_data_maps_root_fields`

### Feature Test Guidelines
- One file per controller — e.g., all `TrucareController` endpoints in `TrucareControllerTest.php`.
- Correct namespace — must match the directory (e.g., `Tests\Feature\Trucare`).
- **Imports:** Remove all direct class paths in the code and always import them at the top of the file.
- Use `RefreshDatabase` — DB is reset between tests; seed rows as needed.
- Mock external service calls — use `$this->mock(ServiceClass::class)` to prevent real HTTP/third-party calls.
- Test via the HTTP layer — use `postJson()`, `actingAs()`, assert on HTTP status and JSON response shape.
- Use `/** @test */` annotation — no `test_` prefix on method names.
- No section-banner comments — method name is the only identity needed.
- Tests that require DB persistence belong here — not in unit tests.

### Feature Test File & Folder Structure
Follow this folder structure exactly:
```
tests/Feature/
  Trucare/        → e.g., TrucareControllerTest.php
  Controllers/    → e.g., CustomerDocumentControllerTest.php
```
- One test file covers **all endpoints** of a single controller — do not split a controller across multiple files.
- Group files by product/domain in a subfolder when the controller belongs to a specific product (e.g., `Trucare/`, `Motor/`).
- Flat controller tests go directly under `tests/Feature/Controllers/`.

### Feature Test Function Naming
Function names must be **snake_case**, fully descriptive, and readable as a sentence describing the **HTTP scenario and expected HTTP response**. Follow this pattern:
```
<endpoint_action>_<scenario>_returns_<http_status_or_outcome>
```
**Examples from existing tests:**
- `get_proposal_unauthenticated_request_returns_401`
- `get_proposal_missing_id_field_returns_422`
- `get_proposal_valid_base64_id_returns_200_with_proposal`
- `issue_policy_valid_proposal_id_returns_200_with_prop_id_and_redirect_url`
- `issue_policy_portal_uw_reason_is_set_to_1_and_saved_to_database`

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
