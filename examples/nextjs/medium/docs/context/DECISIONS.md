# Architectural Decision Records (ADR)

## ADR 1: Technical Stack Foundation
- **Status:** Approved
- **Context:** Several squads share one product and need a single web runtime with strong typing.
- **Decision:** Selected Next.js (App Router) and TypeScript as the only UI runtime. A monorepo (`apps/web` and `packages/*`) is allowed. A separate backend is not created for structure's sake.
- **Consequences:** Team must enforce compilation and type checks at build time. A new service requires a new ADR.

## ADR 2: Data Persistence
- **Status:** Approved
- **Context:** Relational data with transactional consistency is required, and more than one version of the app may be deployed during a release.
- **Decision:** Selected PostgreSQL everywhere tests and production run. Schema changes follow expand, migrate, contract.
- **Consequences:** A migration that rewrites a hot table needs an explicit plan in the pull request. Do not use a different engine locally.

## ADR 3: Layers only for complex domains
- **Status:** Approved
- **Context:** Some flows have real invariants. Most screens do not.
- **Decision:** CRUD stays in feature folders. Contexts listed in [name them] use domain, application, and infrastructure. Tactical DDD stays inside those contexts.
- **Consequences:** Do not mass-rename CRUD features into Clean Architecture. CQRS and event sourcing are not approved.

## ADR 4: Ports for vendors
- **Status:** Approved
- **Context:** Payment, email, and storage vendors change and fail.
- **Decision:** Those integrations are interfaces implemented in infrastructure. Domain code sees results, not SDK types.
- **Consequences:** Swapping a vendor touches one adapter and its tests.
