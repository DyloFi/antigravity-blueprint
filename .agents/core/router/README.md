# Tool Router

Responsibility: choose the right execution surface for an action.

Routing rules:
- prefer local files and commands for repo facts
- prefer MCP tools for configured external systems
- prefer official sources for current API/product behavior
- request approval before privileged, destructive, or networked actions when
  required by the environment

The router must preserve observability: external actions are logged during
`auto-memory` when they are not visible in git diff.
