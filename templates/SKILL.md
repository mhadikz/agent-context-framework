# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `PLAN.md` and `BACKLOG.md` to confirm feature alignment.
2. Draft the system architecture changes textually for the user before writing code.
3. Write matching unit tests alongside the implementation.
4. Run the test suite using the exact command in `AGENTS.md`.

## Integration Guardrails
- **API Pattern:** [e.g., Always use JSON:API schemas for error payloads].
- **State Handling:** [e.g., Never mutate global state directly without using dispatchers].
