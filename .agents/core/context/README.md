# Context Composer

Responsibility: rebuild working context for the current task from durable
sources. Context is a cache, not storage.

Selection order:
1. current goal
2. milestone scope and pipeline status
3. relevant decisions and memories
4. current files
5. recent tool output only when directly relevant

The composer should return the smallest context that changes the next action.
If more than five memory entries seem relevant, narrow the query.
