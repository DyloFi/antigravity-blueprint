---
name: compact-memory
description: Periodically tier and prune warm memory so memory-decisions.md stays small, useful, and mergeable.
---
# Compact-Memory Command

## Step 0 — Confirm scope
Read `.agents/docs/memory-lifecycle.md` and
`.agents/memory-decisions.md`.

Do not compact if there are unresolved merge conflicts, uncommitted user work
you do not understand, or fewer than roughly 20 real memory entries unless the
user explicitly asked for compaction.

## Step 1 — Classify entries
For each older entry, classify it as:
- **keep warm**: still affects current daily work
- **promote**: durable architecture decision, move to
  `.agents/architecture-decisions/`
- **archive**: useful history, move or summarize into `.agents/memory/archive/`
- **discard from warm**: obsolete task detail with no future value

Do not delete provenance. If a decision is promoted or archived, leave a short
pointer in the compaction entry.

## Step 2 — Write cold records first
Create ADR-style markdown files in `.agents/architecture-decisions/` for
promoted decisions. Use this shape:
```
# ADR YYYY-MM-DD: Title

## Status
accepted | superseded | archived

## Context
...

## Decision
...

## Consequences
...

## Provenance
- `.agents/memory-decisions.md` entry: YYYY-MM-DD title
```

Archive session-specific lessons under `.agents/memory/archive/` only when
they remain useful enough to keep.

## Step 3 — Rewrite warm memory carefully
Keep the header and newest useful entries. Add one newest compaction entry that
lists what moved where.

Never rewrite `AGENTS.md` during compaction unless a hot convention actually
changed.

## Step 4 — Verify
Run `git diff --check`.
Then run `pre-memory-verify` if compaction changed command behavior or project
contracts.
