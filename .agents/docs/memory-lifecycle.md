# Memory Lifecycle

The memory system has three temperatures:

- **Hot**: `AGENTS.md` and `.agents/pipeline-status.md`. Small, read every
  session, only current operating facts.
- **Warm**: `.agents/memory-decisions.md`. Recent decisions and lessons that
  still affect daily work.
- **Cold**: `.agents/architecture-decisions/`, `.agents/memory/archive/`, and
  Mem0/MCP. Durable or historical knowledge that should not bloat hot context.

## Write Rules
- Store successes only after verification has run.
- Store failed attempts when the failure teaches the next session something.
- Do not store routine task chatter.
- Prefer short extracted lessons over transcripts.
- Every durable memory needs provenance: user request, file diff, tool output,
  research brief, or external action.

## TTL Guidance
- **Ephemeral task memory**: keep only during the task; archive or discard at
  session end unless it explains a failure.
- **Session lessons**: keep in warm memory while they may affect near-term
  work; compact once stale.
- **Architecture decisions**: promote to `.agents/architecture-decisions/`
  when they outlive the current milestone.
- **Superseded decisions**: move to archive or mark superseded; do not silently
  overwrite.

## Compaction Trigger
Run `compact-memory` when any of these is true:
- `.agents/memory-decisions.md` exceeds roughly 20 real entries.
- A milestone closes.
- Merge conflicts repeatedly involve memory files.
- Recent sessions keep re-reading old entries that no longer change decisions.

Compaction must preserve provenance and should leave a short index in warm
memory pointing to promoted or archived records.
