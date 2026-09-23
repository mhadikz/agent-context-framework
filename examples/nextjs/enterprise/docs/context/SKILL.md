# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `docs/context/PLAN.md`, `docs/context/BACKLOG.md`, and `docs/context/DECISIONS.md`. Confirm the owning context and whether a contract change is additive.
2. Draft the change for the user: invariant location, authz policy, audit event, and compatibility with the currently deployed version.
3. Write unit tests for the rule, integration tests for persistence, and a contract test if another team consumes the route or event.
4. Implement through the context's application service. Do not query another context's tables.
5. Run the commands in `AGENTS.md`. Do not call the work done until it can be deployed, monitored, and rolled back.

### Extraction routine
Do not start this unless an ADR names the trigger. The ADR must say what fails independently, who deploys it, how data moves, how calls are authenticated, and how to roll back.

## Integration Guardrails
- **API Pattern:** Versioned, additive contracts and the error envelope in `docs/context/CONTRACT.md`. Breaking changes are a new version.
- **State Handling:** Personalized responses are not stored in a public cache. Client components do not enforce rules the server must guarantee. Retries are not stacked across the browser, the app, and the downstream client.
