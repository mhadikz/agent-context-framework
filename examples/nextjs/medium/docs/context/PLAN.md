# Macro Project Blueprint

## High-Level Vision
[Describe the ultimate goal of the product]. One Next.js product, several squads, module boundaries that match the business, and a release process that can roll back.

## Core Goals
- [Goal 1: Ship the core domains with invariants enforced in one place and in the database].
- [Goal 2: Give every squad a feature or package they can change without editing another squad's internals].
- [Goal 3: Run lint, types, unit tests, integration tests, and a staging deploy from CI].

## Out of Scope (Non-Goals)
- A microservice per feature, event sourcing, and CQRS command stacks for CRUD.
- Kubernetes, a service mesh, and multi-region active-active as part of feature work.
- Rewriting stable CRUD screens into hexagonal folders.
- [Non-Goal: Native mobile applications, unless a later ADR adds them].

## Key Milestones
- [ ] Phase 1: Monorepo boundaries, PostgreSQL, authn and authz, error envelope, CI.
- [ ] Phase 2: Complex domains extracted into packages with ports for vendors.
- [ ] Phase 3: Traces, feature flags with owners, and runbooks for deploy and rollback.
