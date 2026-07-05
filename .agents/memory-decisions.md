# Evolving Project Decisions & Lessons

> Append-only. Newest entries at the top. One entry per decision/fix.
> Format:
> ## [YYYY-MM-DD] Short title
> - **Context**: what was happening
> - **Decision/Fix**: what was done
> - **Note**: anything future-you needs to not repeat the mistake

## [TEMPLATE] Example entry — delete once you have real entries
- **Context**: Next.js dev server couldn't reach Supabase locally.
- **Decision/Fix**: Confirmed `.env.local` had the correct project URL/anon key; restarted dev server (Next.js only reads env vars at boot).
- **Note**: Always restart `pnpm dev` after editing `.env.local`.
