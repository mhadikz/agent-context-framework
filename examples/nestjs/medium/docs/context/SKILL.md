# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `docs/context/PLAN.md`, `docs/context/BACKLOG.md`, and `docs/context/DECISIONS.md`. Decide whether the module stays flat or uses layers.
2. Draft the facade and the transaction boundary for the user before writing code.
3. Write unit tests for domain rules and integration tests against PostgreSQL. Include an authorization failure.
4. Implement the use case once. Call it from the controller, the webhook, and the worker.
5. Run the commands in `AGENTS.md`. Update OpenAPI if `/v1` changed.

### Schema change routine
1. Expand so the previous release still runs.
2. Backfill and switch readers and writers.
3. Contract in a later release.

## Integration Guardrails
- **API Pattern:** `/v1` uses the error envelope in `docs/context/CONTRACT.md`. Additive changes are the default.
- **State Handling:** Another module is reached through its exported application service. Vendor SDKs stay in infrastructure. In-process events have an explicit subscriber list.
