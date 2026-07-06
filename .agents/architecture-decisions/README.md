# Architecture Decisions

Long-lived decisions belong here once they are stable enough to leave warm
memory.

Use ADR-style files:

```
YYYY-MM-DD-short-title.md
```

Each file should include:
- status
- context
- decision
- consequences
- provenance

Keep `.agents/memory-decisions.md` focused on recent working memory. Promote
durable decisions here during `compact-memory`.
