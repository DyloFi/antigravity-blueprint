# Evolving Project Decisions & Lessons

> Append-only. Newest entries at the top. One entry per decision/fix.
> Format:
> ## [YYYY-MM-DD] Short title
> - **Context**: what was happening
> - **Decision/Fix**: what was done
> - **Note**: anything future-you needs to not repeat the mistake

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
