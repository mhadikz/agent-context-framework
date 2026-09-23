# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `docs/context/PLAN.md`, `docs/context/BACKLOG.md`, and `docs/context/DECISIONS.md`. Name the owning context and the contract impact.
2. Draft invariant location, authz policy, audit event, and how old and new versions behave during rollout.
3. Write domain unit tests, persistence integration tests, and a contract test when another team consumes the API or event.
4. Implement through the application service. Publish cross-process events only via the outbox.
5. Run the commands in `AGENTS.md`. Confirm health, shutdown, and rollback notes exist for the change.

### Service extraction routine
Start only from an approved ADR. It must state the trigger, data ownership, authentication between services, and rollback. Until then, keep the module boundary in-process.

## Integration Guardrails
- **API Pattern:** Versioned additive contracts and the error envelope in `docs/context/CONTRACT.md`.
- **State Handling:** No shared tables across services. Consumers are idempotent. Retries exist at one layer only. Audit records are not debug logs.
