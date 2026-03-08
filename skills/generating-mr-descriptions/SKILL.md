---
name: generating-mr-descriptions
description: Generates professional GitLab Merge Request titles and descriptions from commit hashes and a ticket URL. Use when the user wants to create an MR description based on commits or asks for GitLab MR text.
---

# GitLab MR Description Generator

## When to use this skill
- The user provides commit hashes and a ticket URL and asks for an MR description or title.
- The user mentions generating a release document, MR details, or GitLab MR text from commits.

## Workflow
1.  **Receive Input**: Get one or more commit hashes and a ticket URL from the user.
2.  **Analyze Commits**: Use git commands (e.g., `git show <hash>` or `git log -p <hash>`) to infer the changes made in the provided commits.
3.  **Extract Ticket Number**: Detect uppercase prefixes from the ticket URL (like APB, APF, CPB, CPF, SAL, INT, VS, CAP, etc.).
    - Example: `https://domain/APB-111/feature` -> `APB-111`
4.  **Draft Title**: Create a professional, concise GitLab MR title in the required format.
5.  **Draft Description**: Create an enterprise-grade MR description based on the inferred commit changes.
6.  **Format Output**: strictly present ONLY the required output blocks without any conversational filler.

## Instructions

### Output Structure
The output **MUST** contain exactly these two isolated sections. They must be clearly separated and independently copyable (e.g., enclosed within formatting blocks or standard markdown output without extra text). 

DO NOT include explanations or extra text before or after these sections.

**MR TITLE**
```text
<TICKET-NUMBER> - <Short meaningful summary>
```

**MR DESCRIPTION**
```markdown
- **Ticket:** [<TICKET-NUMBER>](<TICKET-URL>)
- **Change:** <High-level contextual summary of the overall change>
- **Details:**
   - <Bullet points detailing specific technical changes>
   - <Use backticks for all code references>
- **Testing Steps:**
   - <Bullet points outlining how to test the changes>
- **Impact:** <Typically "None", unless explicitly known or requested>
```

## Styling & Formatting Rules
- **Labels**: All field labels must be bolded as shown (`**Ticket:**`, `**Change:**`, `**Details:**`, `**Testing Steps:**`, `**Impact:**`).
- **Imports:** Remove all direct class paths in the code and always import them at the top of the file.
- **Code Formatting**: Wrap ALL code references in backticks (`` ` ``).
  - APIs (e.g., `/api/trucare/print/proposal`)
  - Class names (e.g., `TrucareController`)
  - Interfaces (e.g., `TrucareRepositoryInterface`)
  - Filenames (e.g., `TrucareControllerTest.php`, but formatted readably like `TrucareControllerTest`)
  - JSON files (e.g., `openapi.json`)
- **Exclusions**: 
  - DO NOT include any automated file path citations or source references in the generated output (e.g., `(cci:...)`).
  - DO NOT include full file paths.
  - DO NOT include line numbers.
  - DO NOT include extra conversational commentary.
  - DO NOT use emojis.

### Execution Environment & Permissions
- **Docker Setup:** For cases where you need to run commands, you must ask the user "Are you using docker development setup?". If the user confirms they are using it, then all commands (such as artisan, npm, composer, etc.) must be run inside the app container by prefixing them with `bin/docker-dev exec app `.
- **Command Permissions:** 
  - For **read-only commands** (e.g., `git status`, `git log`, `ls`, `cat`), you DO NOT need to ask the user for permission. Execute them automatically by setting `SafeToAutoRun: true` in your tool execution.
  - For **write, delete, or execute commands**, you MUST strictly ask the user for permission.

## Resources
- None