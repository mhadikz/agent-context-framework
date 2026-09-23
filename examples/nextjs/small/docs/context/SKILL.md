# Runbooks & Deterministic Workflows

## Common Routines
### Feature Implementation Routine
1. Read `docs/context/PLAN.md` and `docs/context/BACKLOG.md` to confirm feature alignment, including the non-goals.
2. Draft the feature-folder change for the user before writing code. Name which rules are pure and which writes need a transaction.
3. Add or extend `schema` and `rules` first. Write unit tests for the business rules and the failure paths.
4. Implement the Server Action or Route Handler so it authenticates, authorizes, validates, then calls the rule and the database.
5. Run the test and build commands in `AGENTS.md`.

### Production-readiness routine
1. Confirm migrations are committed and the database enforces the new uniqueness or reference rule.
2. Confirm errors use the envelope in `docs/context/CONTRACT.md` and unexpected failures are logged without secrets.
3. Confirm a protected path checks both who the user is and whether they are allowed.

## Integration Guardrails
- **API Pattern:** Route Handlers use the error envelope and status codes in `docs/context/CONTRACT.md`. Do not return `"Something went wrong"`.
- **State Handling:** Server data is read and mutated on the server. Do not add a client store for it. Do not duplicate a use case in both an action and a route; extract one function.
