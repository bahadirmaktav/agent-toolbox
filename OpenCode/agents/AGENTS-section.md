## Agents

The following subagents are available. Invoke them by @mentioning
or let the primary agent delegate automatically based on context.

### Available Subagents
- **@cpp-explorer**: Explore and understand the codebase before any change
- **@cpp-architect**: Design and evaluate architecture, propose patterns
- **@cpp-refactor**: Analyze code quality, performance, C++17 improvements
- **@cpp-tester**: Build the project and run tests
- **@cpp-devops**: Analyze and improve CI/CD pipeline and build config

---

### HARD RULES — Non-negotiable

- `git commit` and `git push` are FORBIDDEN in any agent or automated step
- Git operations are ONLY performed via the /commit command or an
  explicit user instruction
- After cpp-tester reports results, STOP. Do not proceed to any git operation.
- No agent proceeds to the next step autonomously without user confirmation

---

### Agent Usage Policy

**MANDATORY. No exceptions.**

Before implementing any non-trivial feature or change:
1. ALWAYS invoke @cpp-explorer first. Do not write any code before
   explorer reports back.
2. ALWAYS invoke @cpp-architect after explorer. Do not write any code
   before architect proposes a design.
3. After implementation, ALWAYS invoke @cpp-tester to verify.
4. Before any MR, ALWAYS invoke @cpp-refactor for a quality check.

For bug fixes:
1. ALWAYS invoke @cpp-explorer to locate and understand the problem.
2. Implement the fix.
3. ALWAYS invoke @cpp-tester to verify.

For pipeline or build issues:
1. ALWAYS invoke @cpp-devops.
