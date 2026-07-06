# Memory Stores

This directory is reserved for the v2 memory system. It is intentionally
schema-first and mostly empty until real task-loop usage proves what should be
stored.

Stores:
- `working/`: scratch state for active tasks or sessions
- `semantic/`: durable project facts and decisions
- `episodic/`: dated events, outcomes, incidents, and lessons
- `procedural/`: reusable operating procedures and fixes
- `knowledge/`: relationship records for graph-style retrieval
- `archive/`: stale or superseded records kept for traceability

Current durable memory remains `.agents/memory-decisions.md`. Do not migrate it
until there is an explicit migration command and a rollback path.
