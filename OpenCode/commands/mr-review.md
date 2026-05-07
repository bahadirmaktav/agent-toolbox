---
description: Review the current branch's MR and post findings as a GitLab comment
---

# mr-review

You MUST follow these steps in order. Do not skip any step.

## Step 1 — Gather Context

Run the following commands:

```bash
git branch --show-current
git diff origin/develop...HEAD
git log origin/develop..HEAD --oneline
```

Extract:
- Current branch name
- Ticket ID from branch name
- Full diff of changes
- Commit history

## Step 2 — Fetch Supporting Context

**Fetch Jira ticket** using the Jira MCP tool to understand the intent and acceptance criteria of the change.

**Fetch the MR** using the `opencode-gitlab-plugin` — find the open MR for the current branch targeting `develop`.

If no open MR is found, inform the user and stop.

## Step 3 — Analyze the Diff

Review the diff thoroughly. For each finding, classify it into one of these severity levels:

| Severity | Meaning |
|---|---|
| 🔴 Critical | Must be fixed before merge. Correctness, security, or data integrity issue. |
| 🟡 Warning | Should be addressed. Logic concern, poor error handling, or maintainability risk. |
| 🔵 Suggestion | Optional improvement. Style, naming, or minor refactor. |
| ✅ Positive | Something done well, worth noting. |

**Review checklist — check each of these:**
- Does the implementation match the Jira ticket requirements?
- Are there any obvious logic errors or edge cases not handled?
- Is error handling present where needed?
- Are there any hardcoded values that should be configurable?
- Is the code readable and reasonably self-documenting?
- Are there any redundant or dead code sections?
- Are there missing or insufficient comments on complex sections?
- Do the changes introduce any unintended side effects on other modules?
- Is the scope of changes reasonable, or does it touch unrelated areas?

## Step 4 — Compose Review Comment

Write the review comment in English using this format:

```markdown
## 🤖 Automated Code Review

**Ticket:** <TICKET-ID> — <ticket summary>  
**Branch:** `<branch name>`  
**Reviewed:** <number> files changed, <number> commits

---

### Summary
<2-3 sentence overall assessment of the change.>

---

### Findings

<For each finding:>
**<Severity emoji> <Short title>**  
File: `<filename>` (line ~<line number if identifiable>)  
<Explanation of the issue or observation. Be specific and constructive.>

---

### Checklist
- [ ] Implementation matches ticket requirements
- [ ] Edge cases handled
- [ ] Error handling present
- [ ] No hardcoded values
- [ ] Code is readable
- [ ] No dead code
- [ ] Complex sections commented
- [ ] No unintended side effects
- [ ] Scope is appropriate

---

*This review was generated automatically. Use your judgment before merging.*
```

If there are no findings, still post the comment with the checklist and a positive summary.

## Step 5 — Post Comment to GitLab MR

Use the `opencode-gitlab-plugin` to post the composed review comment to the MR found in Step 2.

## Step 6 — Report to User

```
✓ Review posted to MR !<mr-id>

Findings:
  🔴 Critical: <count>
  🟡 Warning:  <count>
  🔵 Suggestion: <count>
  ✅ Positive: <count>

URL: <mr url>
```
