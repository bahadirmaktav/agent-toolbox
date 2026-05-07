---
description: Stage all changes and create a conventional commit message aligned with branch type
---

# git-commit

You MUST follow these steps in order. Do not skip any step.

## Step 1 — Check Current State

Run the following and analyze the output:

```bash
git status
git diff
```

If there are no changes to commit, inform the user and stop.

## Step 2 — Detect Branch Type and Jira Ticket ID

Extract the Jira ticket ID from the current branch name:

```bash
git branch --show-current
```

Parse the branch name. The supported branch prefixes are:
- `feature`
- `hotfix`
- `bugfix`
- `infra`
- `refactor`
- `release`

Branch format:
`<prefix>/<TICKET-ID>-short-description` or `<prefix>/short-description`

Example: `feature/PROJ-123-add-login` → ticket ID is `PROJ-123`

If no ticket ID can be detected from the branch name, continue without it.

Map branch prefix to allowed commit prefixes:

| Branch Prefix | Allowed Commit Prefixes |
|---|---|
| `feature` | `feat` |
| `hotfix` | `fix` |
| `bugfix` | `fix` |
| `infra` | `ci`, `docs`, `style`, `build`, `test` |
| `refactor` | `ref`, `perf` |
| `release` | any conventional prefix |

## Step 3 — Analyze Changes and Propose Commit Message

Analyze the diff carefully and determine the most suitable commit prefix.

The selected prefix MUST be valid for the current branch type based on Step 2 mapping. If multiple prefixes are valid (for example `infra`, `refactor`, `release`), choose the most accurate one.

**Prefix selection guide:**
| Prefix | When to use |
|---|---|
| `feat` | New feature or functionality added |
| `fix` | Bug fix |
| `ci` | CI/CD pipeline or configuration changes |
| `docs` | Documentation only changes |
| `style` | Formatting, whitespace, no logic change |
| `build` | Build system or dependency changes |
| `test` | Adding or updating tests |
| `ref` | Refactoring, no functional change |
| `perf` | Performance improvement |

**Commit message format:**
```
<prefix>: <short description in English>

<TICKET-ID>
```

Rules:
- First line: `<prefix>: <description>` — imperative mood, lowercase, no period, max 72 chars
- Blank line
- Last line: Jira ticket ID (e.g. `PROJ-123`)
- If no ticket ID was detected, omit the last line

**Present the proposed commit message to the user and wait for confirmation before proceeding.**

Example output:
```
Proposed commit message:

feat: add user authentication endpoint

PROJ-123

Shall I proceed with staging all files and committing?
```

## Step 4 — Stage and Commit (only after user confirms)

```bash
git add -A
git commit -m "<first line>" -m "<TICKET-ID>"
```

If no ticket ID, omit the second `-m` argument.

Confirm to the user: commit was successful, show the commit hash.
