# Memory Core

Responsibility: decide what becomes durable memory, where it belongs, and what
provenance is required.

Memory stores:
- `working`: temporary task/session state
- `semantic`: durable facts and project knowledge
- `episodic`: session outcomes and notable events
- `procedural`: reusable methods, commands, and gotchas
- `knowledge`: graph-like relationships
- `archive`: superseded or cold information retained for traceability

Rules:
- every durable memory needs source/provenance
- do not store raw context just because it was present
- prefer small extracted facts over transcripts
- archive stale memories instead of silently overwriting them
