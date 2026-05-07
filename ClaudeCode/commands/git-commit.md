---
description: Stage all changes, generate a conventional commit message, commit, and push to main
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*)
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

## Step 2 — Analyze Changes and Propose Commit Message

Analyze the diff carefully and determine the appropriate prefix:

| Prefix  | When to use                              |
|---------|------------------------------------------|
| `feat`  | New feature or functionality added       |
| `fix`   | Bug fix                                  |
| `ci`    | CI/CD pipeline or configuration changes  |
| `docs`  | Documentation only changes               |
| `style` | Formatting, whitespace, no logic change  |
| `build` | Build system or dependency changes       |
| `test`  | Adding or updating tests                 |
| `ref`   | Refactoring, no functional change        |
| `perf`  | Performance improvement                  |

**Commit message format:**
```
<prefix>: <short description in English>
```

Rules:
- Imperative mood, lowercase, no period, max 72 chars

**Present the proposed commit message to the user and wait for confirmation before proceeding.**

Example output:
```
Proposed commit message:

feat: add user authentication endpoint

Shall I proceed with staging all files and committing?
```

## Step 3 — Stage and Commit (only after user confirms)

```bash
git add -A
git commit -m "<commit message>"
```

Confirm to the user: commit was successful, show the commit hash.

## Step 4 — Push to Main

```bash
git push origin main
```

Confirm to the user: push was successful.
