# Macro Project Blueprint

## High-Level Vision
[Describe the ultimate goal of the API]. One NestJS service a small team can deploy, test, and hand to another engineer.

## Core Goals
- [Goal 1: Ship the primary resources with authentication, authorization, and database constraints].
- [Goal 2: Keep business rules out of controllers and covered by tests].
- [Goal 3: Deploy from CI with lint, types, tests, and a production build].

## Out of Scope (Non-Goals)
- Microservice transports, a broker, CQRS, event sourcing, and tactical DDD folders.
- A generic repository interface for every table.
- Kubernetes tuning and multi-region deployment.
- [Non-Goal: a product UI inside this repository].

## Key Milestones
- [ ] Phase 1: App module, config validation, PostgreSQL migrations, global pipe and exception filter, health checks.
- [ ] Phase 2: Feature modules for the core resources, with rule tests and one e2e path.
- [ ] Phase 3: Structured logs, OpenAPI export, and a README for run, test, configure, migrate, and deploy.
