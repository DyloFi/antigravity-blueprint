# Antigravity Blueprint v2 Cognitive Architecture

This template is evolving toward a model-agnostic cognitive architecture for
long-lived engineering agents. The LLM is treated as a replaceable reasoning
engine. Durable intelligence lives in memory, provenance, explicit state, and
repeatable workflows.

## Principles
- **Model agnostic**: commands and schemas should work across Codex, Claude,
  Gemini, Cursor, and future agents.
- **Memory first**: persistent memory is the source of continuity; context is
  rebuilt from memory and project files per task.
- **Context is a cache**: never preserve stale working context as if it were
  durable truth.
- **Modular**: planner, memory, context, retrieval, state, reflection, critic,
  router, and orchestration each have one job and can be replaced.
- **Observable**: plans, state changes, memory writes, and infra actions leave
  breadcrumbs with provenance.

## Loop
```
USER
  -> intent interpreter
  -> goal decomposition
  -> planner
  -> cognitive state machine
  -> observation + tool router + memory
  -> working memory
  -> context composer
  -> LLM reasoner
  -> reflection
  -> memory consolidation
  -> semantic + episodic + procedural memory + knowledge graph
```

## Staged Build
1. **Core cognitive loop**: task-loop, compose-context, planner, state,
   critic, reflection, and observable checkpoints.
2. **Memory system**: working, semantic, episodic, procedural, knowledge, and
   archive stores with schemas, lifecycle rules, branch-local memory, and
   compaction.
3. **Retrieval and knowledge layer**: ranking, graph traversal, context
   budgeting, and memory selection.
4. **Execution layer**: tool routing, MCP boundaries, checkpoints, recovery,
   bounded helper fanout, worktree isolation, MCP boundaries, checkpoints,
   recovery, and external action logs.
5. **Developer experience**: diagnostics, examples, migration guides, tests,
   and reference implementations.

## Current Implementation Status
This repo currently implements stage 1 as command files and documentation. The
stage 2 directory structure and resilience contracts exist as documentation
only. Do not build retrieval, knowledge-graph automation, mandatory hooks, or
a custom MCP server until the core loop has been exercised on a real project
and its friction has been logged in `.agents/memory-decisions.md`.

For sub-agent and parallel-worker decisions, follow
`.agents/docs/agent-orchestration.md`: keep fanout bounded, prefer worktrees
for parallel file edits, and treat unverified Antigravity internals as research
questions rather than project facts.
