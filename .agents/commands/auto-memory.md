---
name: auto-memory
description: Run manually at the end of a real working session (one that started with session-start). Do not run on casual/chat threads with no marker file.
---
# Auto-Memory Command

## Step 0 — Confirm this session counts
Check for `.agents/.session-active`. If it doesn't exist, stop and tell
the user this looks like a casual thread — no session-start was run, so
there's nothing to log. (There is no automatic Stop-hook trigger for
this — Antigravity does not reliably invoke Stop hooks. See the
2026-07-05 memory-decisions.md entry. This command must be run by hand.)

## Step 1 — Diff the session
Run: `git diff` and `git log -5 --oneline`
Read the actual changes, not just file names.

Also explicitly check for anything this session did that a code diff
won't show: a new git remote, a new system-level (Homebrew/pip)
dependency, a manually-edited config file outside the repo, an external
service connected. If anything like that happened, it goes in the dated
entry as its own bullet — "every action should be explainable" includes
infra actions, not just code.

If this session touched `.agents/memory/*`, `.agents/knowledge/*`, or
`.agents/sessions/*`, check that each new record cites provenance and follows
the relevant schema in `.agents/schemas/`.

## Step 2 — Pre-memory verification
Run `pre-memory-verify` (`.agents/commands/pre-memory-verify.md`) before
writing a success entry.

If verification is clean, continue.

If verification fails, do not write this session as complete. Instead, write a
failed-attempt entry with:
- failing command
- short error summary
- likely files involved
- next recommended action

If verification is unavailable, the memory entry must say that explicitly.

## Step 3 — Branch memory check
Run `git branch --show-current`.

If the branch is not `main` and this session is likely to live across multiple
commits or collide with other work, prefer writing the session entry to
`.agents/branches/<safe-branch-name>.md` instead of immediately editing
`.agents/memory-decisions.md`.

On `main`, or for small solo work, write directly to warm memory as usual.

## Step 4 — Write the dated entry
Append to `.agents/memory-decisions.md`, newest at top, using the
existing format:
```
## [YYYY-MM-DD] Short title
- **Context**: what was happening
- **Decision/Fix**: what was done
- **Note**: anything future-you needs to not repeat the mistake
```

If using branch memory, use the format in `.agents/branches/README.md` and do
not duplicate the same entry into `.agents/memory-decisions.md` until merge or
manual consolidation.

## Step 5 — Update AGENTS.md if a convention changed
Only touch AGENTS.md if this session established or revised a
project-wide convention (stack choice, directory rule, anti-pattern).
Routine feature work does not require an AGENTS.md edit.

If the convention is about the v2 agent loop, also update the relevant
contract in `.agents/core/` or `.agents/docs/` so the architecture and command
behavior do not drift apart.

## Step 6 — Run scope-check
Run the `scope-check` command now (see `.agents/commands/scope-check.md`).
Its result gets appended as part of this same dated entry — do not
write two separate entries.

## Step 7 — Memory lifecycle check
If `.agents/memory-decisions.md` is approaching 20 real entries, or if this
session promoted a decision that should outlive the milestone, recommend
running `compact-memory`. Do not run compaction automatically unless the user
asked for it.

## Step 8 — Sync to Mem0 (if connected)
If the Mem0 MCP server is active, push the key facts from this entry so
other projects/sessions can retrieve them cross-project.

## Step 9 — Clean up and report
- Delete `.agents/.session-active`
- Update `.agents/pipeline-status.md` if the stage or Next Action changed
- Report what was written, in 2-3 sentences, before ending the thread
