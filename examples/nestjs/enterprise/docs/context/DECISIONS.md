# Architectural Decision Records (ADR)

## ADR 1: Technical Stack Foundation
- **Status:** Approved
- **Context:** Many teams own different business capabilities behind one platform.
- **Decision:** Selected NestJS and TypeScript. Each capability is a bounded context. Contexts stay in one deployable until an extraction trigger is recorded: independent scale, a hard compliance boundary, or a release cadence the monolith cannot meet.
- **Consequences:** New services without that ADR are out of bounds. Compilation, tests, and contract checks gate the build.

## ADR 2: Data Persistence
- **Status:** Approved
- **Context:** Rolling deploys overlap versions. Extracted services must not couple through tables.
- **Decision:** Selected PostgreSQL. A context owns its tables. Extracted services get their own database. Changes follow expand, migrate, contract.
- **Consequences:** Cross-database foreign keys and shared databases across services are forbidden.

## ADR 3: Hexagonal contexts, flat CRUD
- **Status:** Approved
- **Decision:** Contexts that own invariants use presentation, application, domain, and infrastructure. Administrative CRUD with no invariants stays a flat module.
- **Consequences:** Generating a command handler per endpoint is non-compliant.

## ADR 4: Versioned contracts and outbox
- **Status:** Approved
- **Decision:** Cross-team HTTP is versioned. Events that leave the process are past-tense facts published from a transactional outbox and consumed idempotently.
- **Consequences:** Dual writes to a broker and the database without an outbox are a defect.

## ADR 5: CQRS, event sourcing, and platform controls
- **Status:** Approved
- **Decision:** Add a read model only when the query shape diverges from the aggregate. Use event sourcing only where history is the source of truth. Identity, audit, and OpenTelemetry come from the platform.
- **Consequences:** Default persistence is PostgreSQL state. Local clones of auth or logging are rejected in review.
