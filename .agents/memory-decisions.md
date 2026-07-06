# Evolving Project Decisions & Lessons
<!-- project: antigravity-blueprint -->

> Append-only. Newest entries at the top. One entry per decision/fix.
> Format:
> ## [YYYY-MM-DD] Short title
> - **Context**: what was happening
> - **Decision/Fix**: what was done
> - **Note**: anything future-you needs to not repeat the mistake

## [2026-07-06] Reframed outside setup as Antigravity onboarding
- **Context**: The setup guide still described work as "outside Codex edits,"
  but this template is meant for Antigravity users who may be forking the
  blueprint without knowing which MCPs, skills, rules, or commands are needed.
- **Decision/Fix**: Rewrote `.agents/docs/outside-setup-guide.md` as a
  layman-friendly Antigravity setup guide covering fork/clone, pnpm setup,
  MCP recommendations, built-in command/rule files, daily workflow, worktrees,
  memory compaction, commit/push flow, and what not to automate yet.
- **Note**: No custom MCP server, extra skill package, plugin, or mandatory
  Git hook is required to use the blueprint. Mem0 is recommended for
  cross-project memory; Supabase MCP is recommended only for projects that use
  Supabase.

## [2026-07-06] Imported verified orchestration patterns, not unverified Antigravity internals
- **Context**: Research on Antigravity sub-agents mixed verified platform
  facts with unverified claims about skills, plugin paths, Ultra gating, and
  silent throttling.
- **Decision/Fix**: Added `.agents/docs/agent-orchestration.md`,
  `.agents/commands/prepare-worktree-agent.md`, and
  `.agents/docs/outside-setup-guide.md`. Updated task-loop,
  compose-context, pre-memory-verify, core orchestration/router contracts,
  README.md, AGENTS.md, observability docs, and pipeline status to enforce a
  bounded fanout default, quota-aware planning, worktree isolation for
  parallel file edits, and independent verification/critique.
- **Note**: Do not encode claims like `/plugins/agents/` paths, mandatory
  plugin use, hard Ultra gating, or silent 429 serialization as facts until
  they are verified from official docs or local testing. Treat those as
  research questions.

## [2026-07-06] Added resilience contracts for memory, branches, and verification
- **Context**: External critique identified likely failure modes in the v2
  architecture: merge conflicts in centralized memory, warm-memory bloat,
  success memories written before verification, fragile manual compliance,
  and eventual need for structured tool access.
- **Decision/Fix**: Added `.agents/docs/memory-lifecycle.md`,
  `.agents/commands/pre-memory-verify.md`,
  `.agents/commands/compact-memory.md`, `.agents/branches/README.md`, and
  `.agents/architecture-decisions/README.md`. Updated auto-memory to run
  pre-memory verification before success logging, prefer branch-local memory
  when appropriate, and recommend compaction as warm memory approaches 20
  real entries.
- **Note**: This remains non-destructive and manual-first: no mandatory Git
  hooks, no automatic compaction, no custom MCP server yet. Optional aliases,
  helper scripts, or MCP tools can come later once the manual contracts prove
  their shape.

## [2026-07-06] Blueprint v2 contracts added without speculative automation
- **Context**: A v2 architecture prompt reframed this template as a
  model-agnostic cognitive architecture rather than a prompt library:
  memory-first, context-as-cache, modular, observable, and staged so each
  increment leaves the project usable.
- **Decision/Fix**: Added `.agents/docs/cognitive-architecture.md`,
  `.agents/docs/observability.md`, `.agents/core/*` module contracts,
  `.agents/memory/*` store placeholders, `.agents/knowledge/`,
  `.agents/sessions/`, and JSON schemas for memory records, task state,
  and knowledge edges. Updated session-start, compose-context, task-loop,
  auto-memory, scope-check, AGENTS.md, README.md, and pipeline-status.md
  so the contracts are consulted during real work.
- **Note**: This intentionally does not add retrieval automation, a graph
  engine, or memory-consolidation code yet. Keep stage 2 schema-first until
  the core task-loop has been used on a real target and logged friction
  proves what executable behavior is needed. Also note the repo still
  contains legacy hook files despite an older entry saying they were
  removed; do not rely on hooks for durable behavior.

## [2026-07-06] Stage 1 core loop: task-loop, compose-context added
- **Context**: The v2 architecture discussion called for an explicit
  per-task state machine (PLAN/IMPLEMENT/VERIFY/CRITIQUE/REFLECT) and a
  Context Composer that rebuilds working context per task instead of
  accumulating it. Previously "build" was just an unstructured loop
  between session-start and auto-memory with no visible state and no
  deliberate context-rebuild step.
- **Decision/Fix**: Added `.agents/commands/task-loop.md` (the state
  machine, one run per feature/fix — same granularity as a thread) and
  `.agents/commands/compose-context.md` (read-order + exclusion algorithm,
  run as PLAN's first action). CRITIQUE is deliberately distinct from
  `scope-check`: critique checks the work against its own plan, right
  now; scope-check checks the cumulative session diff against
  Decided/Deferred, at session end. REFLECT is per-task but doesn't write
  to disk directly — it surfaces a candidate line that `auto-memory`
  picks up once per session, so memory-decisions.md doesn't turn into
  one entry per task.
- **Note**: Task-loop has not been run on a real task yet. Don't build
  the retrieval/knowledge-graph layer (stage 3 of the original v2 plan)
  until this loop has actually been used and REFLECT has surfaced real
  friction — the whole point of building in stages was to let usage,
  not the diagram, decide what stage 2/3 need to contain.

## [2026-07-06] Removed dead Stop hook; ported pipeline commands in-repo; added scope-check
- **Context**: `.agents/hooks.json` still wired up the Stop hook even though
  the 2026-07-05 entry below had already proven it never fires. Separately,
  `session-start`, `auto-memory`, and `bootstrap-project` only existed as
  global Antigravity config outside this repo — meaning the pipeline
  wasn't portable to other tools (Claude Code, Codex CLI, Gemini CLI) and
  wasn't visible to anyone who just cloned the template. `scope-check` was
  referenced in AGENTS.md's session protocol but never actually existed
  as a command.
- **Decision/Fix**: Deleted `.agents/hooks.json` and `.agents/hooks/`
  entirely rather than leaving disproven config in place. Created
  `.agents/commands/session-start.md`, `auto-memory.md`, and
  `bootstrap-project.md` as real in-repo commands mirroring the existing
  `define-concept.md` style. Created `.agents/commands/scope-check.md`,
  wired as the last step of `auto-memory`, to compare each session's git
  diff against Decided/Deferred and flag undeclared scope before it gets
  logged as settled.
- **Note**: The pipeline is now fully in-repo and tool-agnostic in
  principle, but this has not been exercised on a real project yet —
  `pipeline-status.md` still shows STAGE: bootstrap-project,
  STATUS: not-started. Don't add further pipeline commands (planner,
  critic, retrieval, etc.) until this version has run end-to-end at
  least once against a real target and the friction from that run is
  known, not assumed.

## [2026-07-05] Corrected false Stop-hook claim; added Rules + verify Workflow
- **Context**: AGENTS.md claimed auto-memory "also runs automatically via the
  Stop hook, but ONLY if .agents/.session-active exists." This was tested
  rigorously (multiple clean Cmd+Q restarts, debug-log redirect on the hook
  command) and found false — Antigravity never invokes the hook at all,
  regardless of the marker file. check-session-active.sh itself is correct;
  the platform just doesn't call it.
- **Decision/Fix**: Corrected the AGENTS.md line to state auto-memory is
  manual-only. Separately added `.agents/rules/typescript-standards.md`
  (glob-activated on `{app,components,lib}/**/*.{ts,tsx}` — this repo has no
  `/src` dir, so the generic `src/**/*.ts` example glob would silently never
  match) and `.agents/workflows/verify.md` (`/verify` — build + lint only,
  no test step since package.json has no test script yet).
- **Note**: Don't add a hook-based auto-approval mechanism for anything
  load-bearing — use Antigravity's Tool Permission presets
  (`proceed-in-sandbox`) instead, since hooks in this platform have a track
  record of firing unreliably or not at all. Also: a later commit
  (858d3e8) reverted hooks.json to a flat `{"Stop": [...]}` structure with
  `"decision": "block"` — this is the exact same schema already tried and
  disproved in an earlier session (see the three-contradictory-answers
  saga). Don't treat this as fixed without seeing the debug log actually
  written.

## [TEMPLATE] Example entry — delete once you have real entries
- **Context**: Next.js dev server couldn't reach Supabase locally.
- **Decision/Fix**: Confirmed `.env.local` had the correct project URL/anon key; restarted dev server (Next.js only reads env vars at boot).
- **Note**: Always restart `pnpm dev` after editing `.env.local`.
