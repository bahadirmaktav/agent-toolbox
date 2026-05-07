---
description: Create and switch to a conventional branch from command arguments without Jira
---

# git-branch

Create a branch directly from command arguments. Do not ask for Jira. Do not inspect conversation history.

You MUST follow these steps in order.

## Step 1 — Read Arguments

Use only `$ARGUMENTS` as the task summary.

If `$ARGUMENTS` is empty or too vague to classify, ask one concise question:
`What are you about to implement/fix in one sentence?`

Do not use conversation history for branch generation.

## Step 2 — Select Branch Type

Use exactly one of these branch types:
- `feature`
- `hotfix`
- `bugfix`
- `infra`
- `refactor`
- `release`

Classification rules:
- New capability, new endpoint, new UI flow, new module -> `feature`
- Urgent production issue -> `hotfix`
- Non-urgent bug fix -> `bugfix`
- CI/CD, pipeline, docs-only, style-only, tests-only, build/dependency ops -> `infra`
- Refactor, cleanup, architecture/code quality, optimization -> `refactor`
- Release prep/version/changelog/release activities -> `release`

Default fallback when ambiguous: `feature`.

## Step 3 — Generate Slug

Generate an English short description slug from `$ARGUMENTS` (or the single fallback answer).

Slug rules:
- lowercase
- words separated with `-`
- allowed chars: `a-z`, `0-9`, `-`
- target length: 3-6 words
- concise and implementation-focused

Branch format:
```
<branch-type>/<slug>
```

Example:
`feature/add-bulk-user-import`

## Step 4 — Prepare Base Branch

Run and analyze:
```bash
git branch --list develop
git branch --list main
```

Base branch selection:
- If `develop` exists, use `develop`
- Else if `main` exists, use `main`
- Else use current branch

Then run:
```bash
git checkout <base-branch>
git pull origin <base-branch>
```

If pull fails because remote branch does not exist, continue with local base branch.

## Step 5 — Create Branch Immediately

Run:
```bash
git checkout -b <branch-name>
```

If branch already exists, append a numeric suffix and retry:
- `<branch-name>-2`
- `<branch-name>-3`

Stop at first successful creation.

## Step 6 — Report Result

Report only final result:
- selected branch type and short reason
- created branch name
- base branch used

Do not ask push-related questions.
