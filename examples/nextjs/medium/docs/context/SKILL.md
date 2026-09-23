# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `docs/context/PLAN.md`, `docs/context/BACKLOG.md`, and `docs/context/DECISIONS.md`. Confirm whether this feature stays a folder or belongs to a layered context.
2. Draft the boundary for the user before writing code: who owns the invariant, which port is called, and whether the migration is additive.
3. Write unit tests for the rules, plus an integration test for the repository or handler against PostgreSQL. Include an authorization failure.
4. Implement the use case once. Call it from the Server Action, the webhook, or the job. Do not copy it.
5. Run the test and build commands in `AGENTS.md`.

### Schema change routine
1. Expand: add the new column or table so old code still works.
2. Migrate: backfill and switch reads and writes.
3. Contract: remove the old shape in a later release.
4. Do not drop or rename a live column in the same release that stops writing it.

## Integration Guardrails
- **API Pattern:** Public Route Handlers use the error envelope in `docs/context/CONTRACT.md` and keep additive compatibility.
- **State Handling:** Vendor SDK calls stay inside infrastructure adapters. UI and domain code do not import them. Server state is not copied into a client store.
