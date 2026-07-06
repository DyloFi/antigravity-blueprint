# Agent Orchestration Contract

Use sub-agents as an execution detail, not as the core architecture. The
portable contract is: one orchestrator owns the task loop, bounded helpers do
specific work, and verification decides what becomes memory.

## Verified Platform Facts
- Antigravity 2.0 supports multiple agents, dynamic subagents, parallel
  workflows, sandboxing, credential masking, and hardened Git policies.
- Antigravity SDK and Managed Agents expose agent harnesses that can run in
  isolated environments.
- Google AI Ultra has higher Antigravity usage limits than Pro.
- Git worktrees support multiple working trees attached to one repository.

## Unverified Claims
Do not build project rules around these until verified from official docs or a
direct local test:
- Skills are always synchronous and unable to run background work.
- Custom sub-agents must live in a specific `/plugins/agents/` path.
- Non-Ultra plans hard-block custom multi-agent orchestration.
- Antigravity silently serializes sub-agents when quota pressure is predicted.

## Fanout Budget
Default to:
- one orchestrator
- zero to two ephemeral helpers

The two useful default helpers are:
- **QA/browser helper**: visual, runtime, or browser verification
- **critic/verify helper**: build, lint, tests, scope, and contract checks

Use more than two helpers only when all are true:
- the work can be split by clear file or domain boundaries
- each helper has independent verification criteria
- merge risk is low or each helper has its own worktree
- the user explicitly approves the extra fanout

## Quota-Aware Planning
Before spawning helpers, ask:
- Can a single agent do this safely with lower coordination cost?
- Will helper output be shorter than the context it consumes?
- Is there a clear fan-in point?
- What happens if a helper fails, times out, or returns partial work?

If the answer is fuzzy, stay sequential.

## Independent Evaluation
The implementer should not be the only judge of its work. Treat verification
and critique as separate roles even when one model performs both.

Minimum independent checks:
- run the repo verify workflow
- compare the diff against `AGENTS.md`
- compare agent-loop changes against `.agents/core/*` and `.agents/docs/*`
- log failed attempts honestly instead of writing success memory

## Execution Layers
- **Skills**: repeatable procedures and domain habits.
- **Worktrees**: parallel local sandboxes with branch-local memory.
- **Plugins / SDK / MCP**: future structured execution layers.

Prefer skills and worktrees now. Add plugins, SDK harnesses, or custom MCP
tools only after the manual contracts prove the API shape.
