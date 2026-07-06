# Branch Memories

Branch memory prevents parallel branches from fighting over
`.agents/memory-decisions.md`.

## When To Use
Use branch memory when:
- multiple developers or agents are working on different branches
- a branch has several sessions before merge
- memory changes would otherwise cause noisy conflicts

For solo, short-lived work on `main`, normal `auto-memory` is enough.

## File Shape
Create one file per branch:

```
.agents/branches/<safe-branch-name>.md
```

Use this format:
```
# Branch Memory: <branch-name>

## Started
- Date:
- Base commit:
- Goal:

## Entries
### YYYY-MM-DD Short title
- Context:
- Decision/Fix:
- Verification:
- Provenance:

## Merge Notes
- Promote to warm memory:
- Archive:
- Discard:
```

## Merge Rule
On merge, do not blindly concatenate branch memories into
`.agents/memory-decisions.md`. Summarize only the decisions that should survive
the branch and include a pointer back to this file or the merge commit.

Mandatory Git hooks are intentionally not part of this contract. Add optional
helper scripts later only after the manual flow proves useful.
