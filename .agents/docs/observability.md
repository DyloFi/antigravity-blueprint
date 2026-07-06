# Observability Contract

Every meaningful agent action should be answerable later:
- What was the goal?
- What state was the loop in?
- What context was used?
- What files, tools, or services were touched?
- What changed, and why?
- What memory was written, with what provenance?

## Breadcrumbs
- `.agents/pipeline-status.md` records current stage and in-flight task state.
- `.agents/memory-decisions.md` records durable decisions and lessons.
- `.agents/sessions/` is reserved for future per-session traces.
- `.agents/memory/*` is reserved for future typed memory stores.

## External Actions
Log any action not visible in `git diff` during `auto-memory`, including:
- git remote changes
- package manager, Homebrew, pip, or system dependency changes
- MCP/server/tool configuration changes
- external service setup
- credentials placement, without recording secret values

## Hook Warning
Do not rely on Stop hooks for durable behavior. Prior sessions found the
Antigravity Stop hook unreliable, and current files may still contain legacy
hook config for investigation. Commands must remain manually runnable and
portable.
