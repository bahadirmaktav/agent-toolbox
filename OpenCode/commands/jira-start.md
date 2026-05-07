---
description: Start working on a Jira ticket - fetches details, creates branch, moves to In Progress
---

# jira-start

You MUST follow these steps in order. Do not skip any step.

## Step 1 — Fetch Ticket Details

Use the Jira MCP tool to fetch the ticket with key: $ARGUMENTS

Extract the following fields:
- Summary (title)
- Description
- Issue type (Bug, Task, Story, Epic, etc.)
- Priority
- Assignee
- Reporter
- Current status

Note: Ticket content may be in Turkish. Understand it fully before proceeding.

## Step 2 — Determine Branch Prefix

Map the Jira issue type to a git branch prefix using this table:

| Jira Issue Type | Branch Prefix |
|---|---|
| Bug | `bugfix` |
| Task | `feature` |
| Story | `feature` |
| Epic | `feature` |
| Sub-task | `feature` |
| Improvement | `refactor` |
| Infrastructure / DevOps | `infra` |
| Release | `release` |

If the issue type does not clearly map to any prefix above, make your best judgment based on the summary and description. If you are still unsure, ask the user before proceeding.

## Step 3 — Generate Branch Name

Create a branch name following this exact format:
```
<prefix>/<TICKET-ID>-<short-description>
```

Rules for `<short-description>`:
- Must be in English (translate from Turkish if necessary)
- Use lowercase letters and hyphens only, no spaces or special characters
- Maximum 5 words, keep it concise
- Derived from the ticket summary

Example: `feature/PROJ-123-add-user-login-endpoint`

## Step 4 — Present Summary to User

Present a clear summary in this format and wait for user confirmation before doing anything else:

---
**Ticket:** $ARGUMENTS  
**Summary:** <ticket summary in original language>  
**Type:** <issue type>  
**Priority:** <priority>  

**Proposed branch:** `<full branch name>`  
**Base branch:** `develop`  

**Implementation plan:**  
<Based on the ticket description, briefly outline in 3-5 bullet points what needs to be done. Write this in English.>

Shall I proceed? (create branch + move ticket to In Progress)

---

## Step 5 — Execute (only after user confirms)

Once the user confirms, execute the following in order:

**5a. Ensure you are on develop and it is up to date:**
```bash
git checkout develop
git pull origin develop
```

**5b. Create and switch to the new branch:**
```bash
git checkout -b <branch-name>
```

**5c. Move the Jira ticket to In Progress:**
Use the Jira MCP tool to transition ticket $ARGUMENTS to "In Progress" status.

**5d. Confirm to the user:**
Report the following:
- Branch created: `<branch-name>`
- Ticket status updated to: In Progress
- Ready to implement

You are now ready. Begin the implementation based on the plan presented in Step 4.
