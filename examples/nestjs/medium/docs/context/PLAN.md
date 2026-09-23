# Macro Project Blueprint

## High-Level Vision
[Describe the ultimate goal of the API]. One NestJS modular monolith that several squads can own by module, with a public contract that does not break consumers casually.

## Core Goals
- [Goal 1: Complex domains enforce invariants in domain code and in PostgreSQL].
- [Goal 2: Squads change a module through its facade without editing another module's persistence].
- [Goal 3: CI runs lint, types, unit tests, integration tests, and an OpenAPI diff].

## Out of Scope (Non-Goals)
- A network service per module, event sourcing, and CQRS for ordinary CRUD.
- A message broker until an ADR names the workload that no longer fits the database.
- Rewriting every flat module into four layers.
- [Non-Goal: a product UI in this repository].

## Key Milestones
- [ ] Phase 1: Module boundaries, config, authz, error envelope, health, CI.
- [ ] Phase 2: Layered modules for [named contexts] and ports for vendors.
- [ ] Phase 3: Traces, `/v1` contract checks, and runbooks for deploy, rollback, and stuck webhooks.
