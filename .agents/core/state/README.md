# State Machine

Responsibility: keep the agent aware of where it is in the loop.

Current states:
```
IDLE -> PLAN -> IMPLEMENT -> VERIFY -> CRITIQUE -> REFLECT -> STORE -> DONE
```

`task-loop.md` currently implements all states except `STORE` as a separate
step. `auto-memory.md` acts as session-level STORE. If task-level storage is
added later, keep it distinct from session memory to avoid noisy logs.
