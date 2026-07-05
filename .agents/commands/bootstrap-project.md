---
name: bootstrap-project
description: Run once, when starting a brand new project from this template. Copies the template in, detects the real stack, and fills in AGENTS.md placeholders.
---
# Bootstrap-Project Command

## Step 1 — Copy the template
Copy this template's contents into the target project directory (the
canonical source is wherever this repo is checked out — e.g.
`~/Templates/antigravity-blueprint/` — do not assume a hardcoded path if
the user's setup differs; confirm the source path if unclear).

## Step 2 — Detect the actual stack
Look for `package.json`, `go.mod`, `Cargo.toml`, or similar. If the
target isn't empty and already has a different stack, do NOT force the
template's Next.js/Supabase defaults — instead:
- Update AGENTS.md's Tech Stack section to reflect what's actually there
- Flag any conflicting convention (e.g. existing CSS framework vs.
  Tailwind) as an Open Question for `define-concept` to resolve, not a
  silent override

## Step 3 — Fill in AGENTS.md placeholders
Replace `[PROJECT NAME]` and `[PROJECT DESCRIPTION]` with real values —
ask the user if not obvious from context (README, package.json name
field, directory name).

## Step 4 — Delete the template example
Remove the `[TEMPLATE]` example entry from `.agents/memory-decisions.md`
if this is a genuinely new project (leave it if bootstrapping is being
re-run on an existing project that already has real entries).

## Step 5 — Update pipeline-status.md
- STAGE: define-concept
- STATUS: not-started
- Next Action: "Run define-concept to lock down the core loop"

## Step 6 — Report
Summarize: detected stack, what was filled in, what was flagged as a
conflict (if anything), and the next command to run.
