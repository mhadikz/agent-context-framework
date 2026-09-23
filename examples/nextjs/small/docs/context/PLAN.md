# Macro Project Blueprint

## High-Level Vision
[Describe the ultimate goal of the product]. Ship it as one Next.js application a small team can run, test, and recover.

## Core Goals
- [Goal 1: Deliver the primary user flow with authentication and server-side authorization].
- [Goal 2: Keep business rules testable outside React, and keep data integrity in PostgreSQL].
- [Goal 3: Deploy from CI with lint, typecheck, tests, and a production build].

## Out of Scope (Non-Goals)
- Microservices, a separate API service, CQRS, event sourcing, and a message broker.
- Clean Architecture or tactical DDD folder structures for CRUD screens.
- A monorepo, a design-system package, Kubernetes, and multi-region deployment.
- [Non-Goal: Native mobile applications].

## Key Milestones
- [ ] Phase 1: App Router shell, config validation, PostgreSQL migrations, authn and authz, error envelope.
- [ ] Phase 2: Core feature folders, rule tests, and the critical end-to-end path.
- [ ] Phase 3: Structured logs, backup notes, and a README that explains run, test, configure, and deploy.
