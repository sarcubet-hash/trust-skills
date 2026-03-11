---
name: database-practices
description: Guidelines for database migrations, schema design best practices, performance, and deployment safety. Use when the user says any of: create migration, add migration, make migration, write migration, add column, add table, create table, add database table, modify schema, update schema, design schema, add foreign key, add index, create model with migration, database design, follow database standards, or any request to create or modify database schema or migrations.
---

# Database Practices and Migration Standards

## When to use this skill
- When the user says: **create**, **add**, **make**, **generate**, **write**, **design**, **modify**, or **update** — and the context involves database/schema/migrations.
- Keyword triggers: `migration`, `schema`, `table`, `column`, `database`, `db`, `foreign key`, `index`, `nullable`, `artisan make:migration`, `artisan make:model -m`.
- When creating new database migrations.
- When designing database schemas or adding new tables.
- When modifying existing columns or relationships.
- When reviewing data migration logic for safety and backward compatibility.
- When the user asks to follow, refer, or use database standards.

## Workflow
- Use the `database-migrations` skill (if available) when explicitly creating migrations.
- Consider deployment safety first—never drop columns or make breaking schema changes in a live environment without a transition plan.
- Ensure proper indexing and relational integrity during the design phase.

## Instructions

### Database Migration Standards
- Table names **must** be `snake_case`.
- Column names **must** be `snake_case`.
- **Imports:** Remove all direct class paths in the code and always import them at the top of the file.

### Deployment Safety & Backward Compatibility
To ensure safe deployments and avoid breaking production during transition:
- **New fields** must be `nullable` or have a default value.
  - *Why?* This prevents application crashes when code is deployed before the database schema is updated.
- **Avoid dropping** or renaming columns directly in a live environment without a transition plan.
- Follow a phased approach for destructive changes:
  1. Add new column.
  2. Update code to use new column.
  3. Migrate data if needed.
  4. Remove old column in a later release.

### Schema Design Best Practices
- **Indexing:** Always use proper indexing for:
  - Foreign keys.
  - Frequently queried columns.
- **Constraints:** Add foreign key constraints where applicable.
- **Data Types:** Use appropriate column types (avoid oversized data types).
- **Foreign Keys:** Use `unsignedBigInteger` for foreign keys (if applicable to the project standard).
- **Enums/Constants:** Avoid storing business logic values as raw strings — use enums or constants.
- **Timestamps:** Add timestamps (`created_at`, `updated_at`) where applicable.
- **Nullability:** Avoid nullable columns unless logically required.
- **Resiliency:** Keep migrations atomic and focused on a single responsibility.
- **Immutability:** **Never** modify existing migrations that are already deployed — create a new migration instead.

### Performance & Integrity
- **JSON:** Avoid unnecessary JSON columns for relational data.
- **Cascading:** Ensure proper cascading rules (`onDelete`, `onUpdate`) are explicitly defined.
- **N+1 Queries:** Review potential N+1 issues caused by schema design.
- **Batching:** Validate large data migrations with batching to prevent memory issues.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
