# Orchestration

Responsibility: coordinate multi-step workflows without hiding state.

Examples:
- task-loop execution
- verification and self-healing
- research-spike confirmation flow
- memory consolidation at session end
- future checkpoint and recovery flows

Keep orchestration thin. Each module should own its domain logic rather than
turning this folder into a catch-all.
