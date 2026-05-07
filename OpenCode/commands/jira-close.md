---
description: Close a Jira ticket - adds worklog comment summarizing the work done and transitions to Done
---

# jira-close

You MUST follow these steps in order. Do not skip any step.

## Step 1 — Gather Context

Run the following commands:

```bash
git log origin/develop..HEAD --oneline
git diff origin/develop...HEAD --stat
```

Also extract the ticket ID:
- If $ARGUMENTS is provided, use that as the ticket ID
- Otherwise, extract ticket ID from current branch name

## Step 2 — Fetch Ticket and MR Details

**Fetch the Jira ticket** using the Jira MCP tool with the identified ticket ID.

Extract:
- Summary
- Description
- Issue type
- Current status

**Fetch the MR** using `opencode-gitlab-plugin` — find the MR for the current branch.

If the MR is still open (not merged), warn the user:
```
⚠️  The MR for this branch is still open and not merged.
    Are you sure you want to close the ticket now?
```
Wait for user confirmation before proceeding.

## Step 3 — Compose Worklog Comment

**Detect the language of the ticket** (summary and description). Write the entire comment in that same language — if the ticket is in Turkish, write in Turkish; if in English, write in English.

Write the comment as plain text only — no markdown, no bold, no headers, no bullet symbols. Jira does not render markdown well. Use simple dashes (-) for list items.

Use this format:

```
Work completed for <TICKET-ID>

Branch: <branch name>
MR: !<mr-id> - <mr url>

Summary:
<3-5 sentence summary of what was implemented. Explain what was done, not how. Reference the original ticket requirements and confirm they were addressed.>

Changes:
- <change 1>
- <change 2>
- <change 3>

Notes:
<Any relevant notes: known limitations, follow-up items, decisions made during implementation. Omit this section entirely if nothing to add.>
```

## Step 4 — Present and Confirm

Show the user the composed comment and ask for confirmation:

---
**Ticket:** <TICKET-ID> — <ticket summary>  
**Action:** Add worklog comment + transition to Done  

**Comment to be posted:**  
<composed comment>

Shall I proceed?

---

## Step 5 — Execute (only after user confirms)

**5a. Post the worklog comment** using the Jira MCP tool — add the comment to the ticket.

**5b. Transition the ticket to Done** using the Jira MCP tool — move ticket status to "Done".

## Step 6 — Report Result

```
✓ Ticket <TICKET-ID> closed successfully

  Comment posted: ✓
  Status: Done

```
