# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `docs/context/PLAN.md` and `docs/context/BACKLOG.md` to confirm feature alignment, including the non-goals.
2. Draft the module change for the user before writing code: DTO, rule, service method, and whether the write needs a transaction.
3. Write unit tests for the rule and for an authorization or validation failure. Add an e2e test if this is a critical write.
4. Implement the controller as HTTP mapping only. The service authenticates the decision, calls the rule, and writes through the database client.
5. Run the test and build commands in `AGENTS.md`.

### Operational routine
1. Confirm `/health/ready` depends on the database.
2. Confirm shutdown hooks are enabled when the process handles in-flight requests.
3. Confirm a new env key is validated at boot and documented in `docs/context/CONTRACT.md` without a real secret.

## Integration Guardrails
- **API Pattern:** Use the status codes and error envelope in `docs/context/CONTRACT.md`. Validation errors are 400. Conflicts are 409 or 422 with a stable code.
- **State Handling:** Do not store use-case state on a singleton besides the database. Do not call a vendor SDK from a controller.
