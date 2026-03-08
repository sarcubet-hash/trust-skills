---
name: managing-git-commits
description: Guidelines for Git commit message formatting, atomic commits, and best practices. Use when committing code to the repository or generating commit messages.
---

# Git Commit Best Practices

## When to use this skill
- When requested to commit code to the repository.
- When generating or formatting git commit messages.
- When organizing codebase changes before creating a Merge Request.

## Workflow
- **Ask the user for the ticket ID** if it is not already provided or cannot find from branch name.
- Review the `git status` or diff to ensure changes are atomic.
- format the commit message precisely according to the `<type>: <ticket_id>` template in all lowercase letters.
- Use present tense verbs for the summary.

## Instructions

### Commit Message Format
Always follow this strict format:
```text
<type>: <ticket_id> - <short description of the change>
```
**Example:**
`feat: apb-144 - add proposal print api validation`

### Formatting Guidelines
- Everything must be written in lowercase letters, including the ticket ID.
- Keep the description short and meaningful.
- Focus on what changed, not how it changed.
- Use present tense (e.g., `add`, `fix`, `update`, `refactor`).
- **Never use vague messages:**
  - `changes`
  - `fix stuff`
  - `update code`

### Best Practices
- **Atomic Commits:** One logical change per commit.
- **Separation of Concerns:** Avoid mixing refactor + feature + formatting in a single commit.
- **Reviewability:** Ensure commits are review-friendly and atomic.
- **Squashing:** Squash unnecessary commits before creating MR (if required by team workflow).
- **History:** Ensure the commit history is readable and useful during debugging.
- **Imports:** Remove all direct class paths in the code and always import them at the top of the file.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None
