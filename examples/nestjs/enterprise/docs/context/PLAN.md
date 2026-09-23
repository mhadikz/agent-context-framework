# Macro Project Blueprint

## High-Level Vision
[Describe the ultimate goal of the platform API]. Bounded contexts that many teams can ship, with compatible contracts and a clear owner for every invariant.

## Core Goals
- [Goal 1: One system of record per capability, with policy and audit on sensitive commands].
- [Goal 2: Rolling deploys stay safe because schema and API changes are backward compatible].
- [Goal 3: Critical journeys have contract tests, traces, an SLO, and a restore path].

## Out of Scope (Non-Goals)
- Microservices, CQRS, or event sourcing adopted because the organization is large.
- Shared databases, shared ORM models, or a large `common/` of business helpers.
- Disabling authorization for internal callers.
- [Non-Goal: products or channels this platform will not provide].

## Key Milestones
- [ ] Phase 1: Context map, platform auth, audit, health, telemetry, and CI scans.
- [ ] Phase 2: Core contexts with versioned `/v1` and compatible migrations.
- [ ] Phase 3: SLOs, on-call runbooks, and a backup restore drill.
