---
description: Create a GitLab Merge Request for the current branch with auto-generated description
---

# git-create-mr

You MUST follow these steps in order. Do not skip any step.

## Step 1 — Gather Context

Run the following commands and analyze the output:

```bash
git branch --show-current
git log origin/develop..HEAD --oneline
git diff origin/develop...HEAD --stat
```

Extract:
- Current branch name
- Ticket ID from branch name (format: `<prefix>/<TICKET-ID>-description`)
- List of commits since develop
- List of changed files

## Step 2 — Fetch Ticket Details

Use the Jira MCP tool to fetch the ticket identified in Step 1.

Extract:
- Summary (title)
- Description
- Issue type

Note: Ticket content may be in Turkish. Understand it fully before proceeding.

## Step 3 — Generate MR Title and Description

**MR Title format:**
```
<prefix>: <short description in English>
```

Use the same prefix as the branch (`feat`, `fix`, `ci`, `docs`, `style`, `build`, `test`, `ref`, `perf`).

**MR Description format:**

```markdown
## What
<What was changed? 2-3 sentences summarizing the implementation. In English.>

## Why
<Why was this change needed? Reference the business reason from the Jira ticket. In English.>

## Changes
<Bullet list of the most important changed files/modules and what was done in each.>

## Jira
<TICKET-ID>
```

## Step 4 — Present Summary and Ask for Confirmation

Present the following to the user and wait for confirmation:

---
**Branch:** `<current branch>`  
**Target:** `develop`  
**Assignee:** you (current GitLab user)  

**MR Title:** `<proposed title>`  

**MR Description:**  
<proposed description>

Shall I create this MR?

---

## Step 5 — Create the MR (only after user confirms)

Use the `opencode-gitlab-plugin` to create the MR with the following parameters:
- **title:** as proposed
- **description:** as proposed
- **source_branch:** current branch
- **target_branch:** `develop`
- **assignee:** current authenticated GitLab user

## Step 6 — Report Result

After the MR is created, report to the user:

```
✓ MR created successfully

Title: <mr title>
URL: <mr url>
MR ID: <mr id>
```

Do NOT open the browser. Just show the link.
