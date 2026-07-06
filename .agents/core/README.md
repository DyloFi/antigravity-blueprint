# Core Modules

These folders define module boundaries for the agentic engineering loop. They
are contracts first, not a mandate to add code before there is evidence it is
needed.

- `planner/`: turns a goal into a small executable plan.
- `memory/`: owns memory lifecycle, consolidation, and provenance.
- `context/`: composes fresh task context from durable sources.
- `retrieval/`: ranks and selects memories or graph nodes for context.
- `state/`: tracks the explicit cognitive state machine.
- `reflection/`: extracts lessons from outcomes.
- `critic/`: checks work against the plan before durable logging.
- `router/`: maps actions to tools, MCP servers, APIs, or local commands.
- `orchestration/`: coordinates multi-step flows and recovery.

Add implementation files only when a command or workflow needs executable
behavior. Until then, keep these folders as documentation boundaries.
