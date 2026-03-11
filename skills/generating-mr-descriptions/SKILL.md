---
name: generating-mr-descriptions
description: Generates professional GitLab Merge Request titles and descriptions from commit hashes and a ticket ID or URL. Use when the user says any of: create MR, generate MR, write MR, make MR description, create merge request, generate merge request, MR description, MR title, GitLab MR, write merge request description, make an MR, create MR for this, or any request that references an MR, merge request, or GitLab MR text generation.
---

# GitLab MR Description Generator

## When to use this skill
- When the user says: **create**, **generate**, **write**, **make**, or **build** — and the context involves an MR, merge request, or MR description/title.
- Keyword triggers: `MR`, `merge request`, `MR description`, `MR title`, `GitLab MR`, `MR text`, `release notes` (when commit hashes are involved).
- The user provides commit hashes and a ticket ID or URL and asks for an MR description or title.
- The user mentions generating a release document, MR details, or GitLab MR text from commits.

## Workflow
1.  **Receive Input**: Get one or more commit hashes from the user. A ticket ID or URL is optional.
2.  **Resolve Ticket URL**: Determine the full ticket URL using the following priority order:

    **a) Full URL provided** — starts with `https://`, use it as-is.

    **b) Ticket ID provided** (e.g., `APB-144`) — automatically construct the full URL:
    ```
    https://trustinsurancecubet.youtrack.cloud/issue/<TICKET-ID>
    ```
    Example: `APB-144` → `https://trustinsurancecubet.youtrack.cloud/issue/APB-144`

    **c) No ticket given** — auto-detect from the current git branch name:
    - Run `git branch --show-current` automatically (`SafeToAutoRun: true`).
    - Extract the ticket ID by matching the pattern `[A-Z]+-[0-9]+` (e.g., `APB-144`, `APF-89`, `CPB-12`) from the branch name.
      - Example branch: `feature/APB-144-add-proposal-api` → ticket ID: `APB-144`
      - Example branch: `apb-144-fix-validation` → ticket ID: `APB-144` (normalise to uppercase)
    - Once extracted, construct the full URL:
      ```
      https://trustinsurancecubet.youtrack.cloud/issue/<TICKET-ID>
      ```
    - If no ticket pattern is found in the branch name, proceed without a ticket URL and leave the `Ticket` field blank in the description.

3.  **Analyze Commits**: Use git commands (e.g., `git show <hash>` or `git log -p <hash>`) to infer the changes made in the provided commits.
4.  **Extract Ticket Number**: Extract the ticket ID from whichever source resolved it (provided ID, URL, or branch name).
5.  **Draft Title**: Create a professional, concise GitLab MR title in the required format.
6.  **Draft Description**: Create an enterprise-grade MR description based on the inferred commit changes.
7.  **Format Output**: strictly present ONLY the required output blocks without any conversational filler.

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