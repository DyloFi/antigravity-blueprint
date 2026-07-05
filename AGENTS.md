# Project Core Map (AGENTS.md)

> Read this file first, every session. Keep it under 200 lines.
> This is a TEMPLATE — after cloning for a new project, fill in the
> [PROJECT NAME], [PROJECT DESCRIPTION], and any stack deviations below.

## Project
- Name: [PROJECT NAME]
- One-line description: [PROJECT DESCRIPTION]
- Status: scaffold / in-progress / shipped

## Tech Stack (default — only deviate if the project needs it)
- Frontend: Next.js 15 (App Router), TypeScript, TailwindCSS, shadcn/ui
- Backend/DB/Auth: Supabase (Postgres, Auth, Storage)
- Hosting: Vercel (frontend + API routes)
- Package manager: pnpm
- Memory: Mem0 (via MCP, configured globally at ~/.gemini/config/mcp_config.json — do not duplicate the key here or in any committed file)

## Directory Conventions
- `/app` — routes and pages (App Router)
- `/components` — shared React components
- `/components/ui` — shadcn/ui primitives (generated, avoid hand-editing)
- `/lib` — utilities, Supabase client, helpers
- `/lib/supabase.ts` — single source of truth for the Supabase client
- `.agents/` — memory system (see below), not app code

## Build & Test Commands
- Install: `pnpm install`
- Dev server: `pnpm dev`
- Build: `pnpm build`
- Lint: `pnpm lint`

## Environment Variables
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- Keep these in `.env.local` (already gitignored). Never commit real keys.

## MCP Servers
Global config lives at `~/.gemini/config/mcp_config.json` (not committed —
contains keys/tokens).
- **Active**: mem0 (cross-project memory), sequential-thinking (free,
  local, no credentials — reasoning aid), supabase (queries/manages your
  Supabase project directly instead of manual SQL)
- **Deferred, not added** (add only if a real project's `define-concept`
  run surfaces an actual need, not speculatively): Linear/Notion (task
  tracking — no multi-person collaboration yet), Netlify (deployment — no
  project ready to deploy yet), MongoDB/Pinecone (Supabase already covers
  DB + vector search), Perplexity web-session workaround (explicitly
  avoided — uses unofficial tooling against account terms)

## Agent Guidelines & Anti-Patterns
- Check `.agents/memory-decisions.md` before making architectural changes or
  re-deciding something that was already decided.
- Don't introduce a second UI kit, CSS framework, or state library — this
  project uses Tailwind + shadcn/ui and React state/hooks unless a
  memory-decisions.md entry says otherwise.
- Don't hand-edit generated shadcn components in `/components/ui` — instead
  regenerate or wrap them.
- Update this file or `.agents/memory-decisions.md` whenever a correction is
  made or a new convention is adopted.
- If Mem0 (MCP) is connected, prefer querying it for cross-project patterns
  before asking the user to re-explain something.

## Session Protocol
These are global Antigravity custom commands (not project-local skills) —
they work automatically in any project cloned from this template:
- Start of session: `run session-start` — reads this file + memory-decisions.md, checks git status, aligns context, and creates `.agents/.session-active` so auto-memory knows this was a real working session.
- End of session: `run auto-memory` (also runs automatically via the Stop hook, but ONLY if `.agents/.session-active` exists — i.e. only if you ran session-start first) — diffs the session, appends a dated entry to memory-decisions.md, updates this file if a convention changed, syncs to Mem0 if connected.
- New project: `run bootstrap-project` — copies this template from `~/Templates/antigravity-blueprint/`, detects the stack, and fills in this file automatically.
- Casual/chat threads: if you never run `session-start`, no marker exists, and the Stop hook does nothing — brainstorming and quick questions don't get logged as project decisions.

## Full Pipeline (for new/extending scope)
1. `run bootstrap-project` — stack + memory skeleton
2. `run define-concept` — locks core loop into Decided/Deferred/Open below;
   forces a preference-vs-research fork on every open item (see
   `.agents/commands/define-concept.md`)
3. `run research-spike` — only for items marked type: research; one
   question per spike, output is a brief in `.agents/research/`
4. `run generate-agent-team` — gated on zero blocking Open Questions;
   proposes smallest viable agent team, confirmed by you before writing
5. `run session-start` / build / `run auto-memory` — normal working loop
6. Scope grows → back to step 2 for the new increment only

Check `.agents/pipeline-status.md` any time you're unsure what stage this
project is at or what to run next.

## Current Milestone Scope
### Decided
- (none yet — run define-concept)

### Deferred
- (none yet)

### Open Questions (BLOCKING)
- (none yet)
