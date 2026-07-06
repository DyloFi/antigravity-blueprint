# Critic

Responsibility: compare the completed work against the plan before it becomes
durable memory.

Checks:
- did the implementation satisfy the stated goal?
- did it drift into undeclared scope?
- were edge cases revealed by the actual diff?
- is the result simpler than the plan implied?
- did verification cover the risky parts?

The critic is not a second full code review. It is a focused drift and quality
check before reflection.
