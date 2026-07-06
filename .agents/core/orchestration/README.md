# Orchestration

Responsibility: coordinate multi-step workflows without hiding state.

Examples:
- task-loop execution
- verification and self-healing
- research-spike confirmation flow
- memory consolidation at session end
- bounded helper fanout
- worktree-agent preparation
- future checkpoint and recovery flows

Keep orchestration thin. Each module should own its domain logic rather than
turning this folder into a catch-all.

Default fanout is one orchestrator plus at most two ephemeral helpers. More
parallelism requires explicit approval, clear ownership boundaries, and a
fan-in plan.
