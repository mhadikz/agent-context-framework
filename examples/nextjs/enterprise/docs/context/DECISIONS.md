# Architectural Decision Records (ADR)

## ADR 1: Technical Stack Foundation
- **Status:** Approved
- **Context:** Many teams share an experience layer and several systems of record.
- **Decision:** Selected Next.js (App Router) and TypeScript for product UI. The owning bounded context remains the system of record. The web app does not keep a second copy of domain rules.
- **Consequences:** Team must enforce compilation, type, and contract checks at build time.

## ADR 2: Data Persistence
- **Status:** Approved
- **Context:** Rolling deploys run old and new code against the same database.
- **Decision:** Selected PostgreSQL. Each context owns its tables. Schema changes follow expand, migrate, contract. A service, once extracted, does not share a database.
- **Consequences:** Cross-context SQL and same-release destructive migrations are defects.

## ADR 3: Modular monolith until an extraction trigger
- **Status:** Approved
- **Context:** Microservices add network failure, tracing, and compatibility cost.
- **Decision:** Contexts ship as packages until independent scale, a compliance boundary, or a separate release cadence requires a split. That split is a new ADR.
- **Consequences:** "Enterprise architecture" is not a reason to add a service.

## ADR 4: CQRS and event sourcing are opt-in
- **Status:** Approved
- **Context:** Those patterns solve specific read-model and history problems.
- **Decision:** CQRS only where the read model is genuinely different. Event sourcing only where history is the business record (ledger, trading, regulated audit).
- **Consequences:** New CRUD is a use case and a table, not a command stack and an event store.

## ADR 5: Platform auth, audit, and design system
- **Status:** Approved
- **Decision:** Use the organization identity and policy library, the shared audit pipeline for sensitive actions, OpenTelemetry, and the design-system package.
- **Consequences:** Feature teams do not fork session handling, logging, or visual primitives.
