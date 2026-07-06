---
name: pre-memory-verify
description: Run before auto-memory writes a success entry. Verifies the repo state and decides whether memory should record success, failure, or an unresolved attempt.
---
# Pre-Memory-Verify Command

## Step 1 — Detect verification workflow
Read `.agents/workflows/verify.md`.

If the repo has no package manager files or verify workflow, report
"verification unavailable" and continue only if the memory entry clearly says
verification was unavailable.

## Step 2 — Run verification
Run the current verify workflow exactly as written.

For this template today, that means:
1. `pnpm build`
2. `pnpm lint`

If a test script is added later, `.agents/workflows/verify.md` must be updated
and this command follows it.

## Step 3 — Classify outcome
- **clean**: auto-memory may write a normal success/decision entry.
- **fixed during verify**: auto-memory may write a success entry, but include
  what verification caught and fixed.
- **failing**: auto-memory must not write the session as complete. Write a
  failed-attempt entry with the exact failing command, a short error summary,
  files likely involved, and the next recommended action.
- **unavailable**: auto-memory may write only if the entry explicitly says no
  verification ran and why.

## Step 4 — Critique before storage
Before memory is written, compare the session diff against:
- `AGENTS.md`
- `.agents/docs/cognitive-architecture.md` when agent architecture changed
- `.agents/docs/agent-orchestration.md` when helpers, sub-agents, worktrees,
  skills, plugins, MCP, or SDK harnesses were involved
- relevant `.agents/core/*/README.md` contracts

If the diff violates a guideline, stop and either fix it or log the memory as
an unresolved/failing attempt. Do not store a clean success narrative for work
that knowingly violates the project map.

The verification/critique role should be independent from implementation in
spirit: it needs evidence from commands, diffs, screenshots, tests, or review
criteria, not just the implementer's assertion that the work is done.
