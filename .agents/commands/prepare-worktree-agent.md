---
name: prepare-worktree-agent
description: Prepare a parallel worktree for an isolated agent task without touching the main working tree.
---
# Prepare-Worktree-Agent Command

Use this only when a task is safe to run in parallel and the user wants a
separate worktree. For ordinary single-thread work, use `task-loop` in the
current worktree.

## Step 0 — Preconditions
Read:
- `.agents/docs/agent-orchestration.md`
- `.agents/branches/README.md`
- `AGENTS.md`

Stop if the current worktree has uncommitted changes you do not understand.
Never discard or move user work.

## Step 1 — Define the isolated task
Write down:
- branch name
- one-sentence task
- files or directories the agent may touch
- files or directories it must not touch
- verification command
- expected branch-memory file

If the task cannot be bounded this tightly, do not create a worktree.

## Step 2 — Create the worktree
Recommended shape:
```
git worktree add ../<repo>-<short-task> -b agent/<short-task>
```

If the branch already exists, stop and ask before reusing it.

## Step 3 — Seed branch memory
In the new worktree, create:
```
.agents/branches/<safe-branch-name>.md
```

Use the template from `.agents/branches/README.md`. Include the base commit
from `git rev-parse HEAD` and the task boundary from Step 1.

## Step 4 — Run the isolated loop
Inside the new worktree:
1. `run session-start`
2. `run task-loop`
3. `run auto-memory`

Prefer branch memory over `.agents/memory-decisions.md` unless the branch is
merged immediately and the decision clearly belongs in warm memory.

## Step 5 — Fan-in
Before merging:
- run verification in the worktree
- review the branch memory
- summarize only durable decisions into main warm memory or ADRs
- remove or archive branch memory when it no longer adds value

Do not blindly concatenate branch memories into `.agents/memory-decisions.md`.
