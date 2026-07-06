# Antigravity Setup Guide

This guide is for someone who wants to fork this blueprint, open it in
Antigravity, and use the agent loop without needing to understand the whole
architecture first.

## What You Need

Required:
- Git
- Node.js
- pnpm
- Antigravity
- A GitHub account

Recommended:
- Mem0 MCP for cross-project memory
- Supabase MCP if the project uses Supabase

Optional:
- sequential-thinking MCP
- shell aliases
- git worktrees for parallel agents

Not required yet:
- custom MCP server
- Antigravity plugin
- extra skill package
- mandatory Git hooks

## Fork And Clone

1. Fork `DyloFi/antigravity-blueprint` on GitHub.
2. Clone your fork:

```bash
cd ~/Templates
git clone https://github.com/<your-username>/antigravity-blueprint.git
cd antigravity-blueprint
```

3. Open the folder in Antigravity.

## First-Time Project Setup

Install dependencies:

```bash
pnpm install
```

Create local environment variables:

```bash
cp .env.local.example .env.local
```

Then fill in:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

Run the app:

```bash
pnpm dev
```

## MCP Setup

You do not need a custom MCP server to use this blueprint.

Recommended MCPs:
- **Mem0**: useful for remembering patterns across projects.
- **Supabase**: useful if your app uses Supabase and you want the agent to
  inspect/manage the project through tools instead of ad hoc SQL.
- **sequential-thinking**: optional reasoning aid.

Keep MCP credentials in Antigravity's global MCP config, not this repository.
For this machine, the expected global location is:

```text
~/.gemini/config/mcp_config.json
```

Never commit API keys, access tokens, or Supabase service-role keys.

## Skills, Rules, And Commands

You do not need to install an extra skill package to use the blueprint.

This repo already carries its operating instructions as files:
- `AGENTS.md`: read every session
- `.agents/commands/`: reusable command protocols
- `.agents/rules/`: project rules
- `.agents/workflows/`: verification workflows
- `.agents/docs/`: architecture and setup guides

In Antigravity, use the command names directly when working with the agent:
- `run session-start`
- `run define-concept`
- `run task-loop`
- `run auto-memory`
- `run compact-memory`
- `run prepare-worktree-agent`

If Antigravity does not auto-detect a command file, tell the agent to read the
matching file in `.agents/commands/` and follow it.

## Normal Workflow

Start a real working session:

```text
run session-start
```

Define the product before building:

```text
run define-concept
```

For each focused feature or fix:

```text
run task-loop
```

Before closing the thread:

```text
run auto-memory
```

`auto-memory` runs pre-memory verification before writing a success entry. If
verification fails, it should log a failed attempt instead of pretending the
session completed cleanly.

## Parallel Agent Work

Do not have multiple agents edit the same folder at the same time.

For parallel work, use a Git worktree:

```bash
cd ~/Templates/antigravity-blueprint
git worktree add ../antigravity-blueprint-<task> -b agent/<task>
```

Open the new folder in Antigravity and run:

```text
run prepare-worktree-agent
```

This creates a bounded task, branch-local memory, and a safer fan-in path.

## Memory Hygiene

Use:

```text
run compact-memory
```

when:
- `.agents/memory-decisions.md` gets long
- a milestone closes
- old session notes no longer affect daily work
- branch memories need promotion or archive

Compaction should promote durable decisions to
`.agents/architecture-decisions/` and archive stale details.

## Commit And Push

After the agent changes files:

```bash
git status --short
git add AGENTS.md README.md .agents
git diff --cached --stat
git commit -m "<short message>"
git push origin main
```

For riskier workflow changes, use a branch:

```bash
git checkout -b agent/<short-topic>
git add AGENTS.md README.md .agents
git commit -m "<short message>"
git push -u origin codex/<short-topic>
```

## Optional Shell Aliases

Aliases are personal convenience only. They are not required.

Example:

```bash
alias ag-status='git status --short --branch'
alias ag-diff='git --no-pager diff --stat'
alias ag-staged='git --no-pager diff --cached --stat'
```

## What Not To Add Yet

Avoid adding these until the manual file-based loop has produced repeated,
specific friction:
- mandatory Git hooks
- custom MCP server
- custom Antigravity plugin
- automated memory compaction
- automated multi-agent fanout

The blueprint's current design is intentionally file-first and portable.
Automate only after the manual loop proves what needs automation.
