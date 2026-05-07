---
description: Create and switch to a conventional branch name from arguments or interactive prompts
---

# git-branch

You MUST follow these steps in order. Do not skip any step.

## Step 1 — Collect Inputs (Argument or Interactive)

Supported branch types:
- `feature`
- `hotfix`
- `bugfix`
- `infra`
- `refactor`
- `release`

If `$ARGUMENTS` is provided, parse it as:
```
<branch-type> <ticket-or-dash> <short-description>
```

Examples:
- `feature JIRA-324 english explanation`
- `bugfix CRM-91 fix login race`
- `infra - update pipeline cache`

Rules:
- `<branch-type>` must be one of the supported values
- `<ticket-or-dash>` is required
  - Jira ticket format example: `PROJ-123`
  - If no Jira ticket, use `-`
- `<short-description>` is required and must be in English (translate if needed)

If `$ARGUMENTS` is empty, switch to interactive mode:
1. Ask user for a short work description (free text).
2. Ask user for Jira ticket ID (optional, user may leave empty).
3. Determine branch type from description with this mapping:
   - new feature, endpoint, new page, new capability -> `feature`
   - urgent production bug fix -> `hotfix`
   - normal bug fix (non-urgent) -> `bugfix`
   - CI/CD, pipeline, devops, docs, style-only, test infra, build config -> `infra`
   - refactor, cleanup, code quality, optimization -> `refactor`
   - release prep/version/release activities -> `release`
4. If confidence is low between two types, ask one concise disambiguation question and proceed.

Jira is optional in both modes.

## Step 2 — Build Branch Name

Generate branch name in conventional format:

- With Jira:
```
<branch-type>/<TICKET-ID>-<short-description>
```

- Without Jira:
```
<branch-type>/<short-description>
```

Rules for `<short-description>`:
- lowercase only
- words separated with `-`
- only `a-z`, `0-9`, and `-`
- concise and meaningful
- prefer 4-6 words where possible

Examples:
- `feature/JIRA-324-english-explanation`
- `infra/update-pipeline-cache`

Jira validation:
- If Jira field is empty or `-`, treat as no Jira
- If provided but not in `ABC-123` style, ask user to correct it or leave empty

## Step 3 — Prepare Base Branch

Run and analyze:
```bash
git branch --show-current
git branch --list develop
git branch --list main
```

Base branch selection:
- if `develop` exists, use `develop`
- otherwise, use `main`

If neither `develop` nor `main` exists locally, explain the issue and ask user which base branch to use.

Then update base branch:
```bash
git checkout <base-branch>
git pull origin <base-branch>
```

## Step 4 — Present Plan and Ask Confirmation

Show this summary and wait for user confirmation:

---
**Base branch:** `<base-branch>`  
**New branch:** `<branch-name>`

**Reasoning:** `<why this branch type was chosen>`

Shall I proceed with creating and switching to this branch?
---

## Step 5 — Create Branch (only after user confirms)

Run:
```bash
git checkout -b <branch-name>
```

If branch already exists:
- propose an alternative by appending a short numeric suffix (example: `-2`)
- ask confirmation, then create the alternative branch

Then confirm:
- current branch name
- ready for commit using matching conventional prefix rules
