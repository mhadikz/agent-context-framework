# Macro Project Blueprint

## High-Level Vision
[Describe the ultimate goal of the product]. An experience layer and bounded contexts that many teams can change without breaking each other's contracts or data.

## Core Goals
- [Goal 1: Each capability has one owner, one invariant location, and one system of record].
- [Goal 2: Public contracts and database changes stay compatible across a rolling deploy].
- [Goal 3: Critical journeys have tests, traces, an SLO, and a rehearsed rollback].

## Out of Scope (Non-Goals)
- Adopting microservices, CQRS, or event sourcing because the company is large.
- A second design system, a forked auth stack, or shared utility packages that every team must take breaking changes from.
- Cross-context database access and same-release destructive schema changes.
- [Non-Goal: specific products or channels this program will not build].

## Key Milestones
- [ ] Phase 1: Context map, platform auth, audit, telemetry, and CI gates (lint, types, tests, scans).
- [ ] Phase 2: Core contexts with versioned contracts and compatible migrations.
- [ ] Phase 3: SLOs, on-call runbooks, and a restore drill for the primary database.
