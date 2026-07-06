# Antigravity Blueprint

Personal starter template: Next.js + TypeScript + TailwindCSS + shadcn/ui +
Supabase, plus a file-based agent memory system for use with Antigravity 2.0.
The agent layer is being shaped into Antigravity Blueprint v2: a portable,
model-agnostic cognitive loop where memory, context composition, explicit
state, critique, and observability matter more than any one LLM.

## What's in here

```
antigravity-blueprint/
├── AGENTS.md                     <- hot memory: project map, read every session
├── app/                          <- Next.js App Router pages
├── components/ui/                <- shadcn/ui-style components
├── lib/
│   ├── supabase.ts                <- single Supabase client instance
│   └── utils.ts                   <- cn() helper for shadcn components
├── .env.local.example             <- copy to .env.local and fill in
└── .agents/
    ├── core/                      <- v2 cognitive module contracts
    ├── docs/                      <- architecture + observability docs
    ├── schemas/                   <- memory/state/knowledge schemas
    ├── memory/                    <- future typed memory stores
    ├── architecture-decisions/     <- promoted long-lived decisions
    ├── branches/                  <- optional branch-local memory
    └── memory-decisions.md        <- warm memory: append-only decision log
```

Session logic (`session-start`, `auto-memory`, `scope-check`) and project
bootstrapping (`bootstrap-project`) live as real command files in
`.agents/commands/`, alongside `define-concept`, `research-spike`, and
`generate-agent-team` — see "Daily workflow" below.

Start with `.agents/docs/cognitive-architecture.md` for the v2 design. The
new `.agents/core/` and `.agents/memory/` folders are contracts first; they are
not an invitation to add speculative automation before real usage shows where
the loop hurts.

## First-time setup (per new project cloned from this template)

1. `pnpm install`
2. `cp .env.local.example .env.local` and fill in your Supabase project URL
   + anon key (Supabase dashboard → Project Settings → API)
3. `pnpm dev` and confirm it runs at localhost:3000
4. Open `AGENTS.md` and fill in the project name/description at the top
5. Delete the template example entry in `.agents/memory-decisions.md`

## Mem0 / MCP

This template assumes Mem0 is already configured globally in Antigravity at
`~/.gemini/config/mcp_config.json` with your API key. Nothing project-specific
needs to be added here — the `session-start` and `auto-memory` skills will
use it automatically if it's connected. Never commit an API key into this
repo.

## Daily workflow (using your global Antigravity commands)

1. **Start of session**: `run session-start`. Reads `AGENTS.md` +
   `.agents/memory-decisions.md`, checks git status, prints a stack/decisions
   summary, and confirms it's ready.
2. **While working**: keep threads focused — start a new chat thread per
   feature/endpoint rather than letting one thread run for hundreds of turns.
3. **End of session**: `run auto-memory`. Diffs the session's changes,
   runs pre-memory verification, appends a dated entry to
   `memory-decisions.md` or branch memory, updates `AGENTS.md` if a
   project-wide convention changed, and syncs key facts to Mem0. Commit the
   markdown changes along with your code.

When warm memory grows, run `compact-memory` to promote durable decisions into
`.agents/architecture-decisions/` and archive stale session detail.

For parallel agent work, prefer `prepare-worktree-agent` over multiple agents
editing one folder. See `.agents/docs/agent-orchestration.md` and
`.agents/docs/outside-setup-guide.md` for the Antigravity setup guide,
including MCP, commands, memory, worktrees, and commit flow.

## Bootstrapping a brand new project from this template

1. Make sure this template lives at `~/Templates/antigravity-blueprint/`
   (macOS: `/Users/dylanboyle/Templates/antigravity-blueprint/`) — this is
   the path your `bootstrap-project` command copies from.
2. Open your new (empty or existing) project folder in Antigravity.
3. `run bootstrap-project`. It copies this template in, detects your actual
   stack from files like `package.json`/`go.mod`/`Cargo.toml`, and rewrites
   `AGENTS.md`'s placeholders with the real stack, commands, and conventions.
4. `run session-start` from then on, like any other project.

If the new project is genuinely a fresh Next.js + Supabase app with no
existing code to detect, `bootstrap-project` will just keep this template's
defaults as-is — confirm the `.env.local` and Supabase keys before you start.
