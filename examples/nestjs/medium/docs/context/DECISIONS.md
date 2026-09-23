# Architectural Decision Records (ADR)

## ADR 1: Technical Stack Foundation
- **Status:** Approved
- **Context:** Several squads share one product API and one transactional database.
- **Decision:** Selected NestJS and TypeScript as one deployable, plus an optional worker process in the same repository. Modules collaborate in-process through facades.
- **Consequences:** A network split requires a new ADR with a scaling, compliance, or release reason. Compilation and type checks are mandatory.

## ADR 2: Data Persistence
- **Status:** Approved
- **Context:** Old and new code may run during a release.
- **Decision:** Selected PostgreSQL everywhere. Schema changes follow expand, migrate, contract. Constraints and transactions enforce invariants the database can express.
- **Consequences:** Breaking DDL in the same release as the code switch is a defect.

## ADR 3: Layers only in complex modules
- **Status:** Approved
- **Context:** Some modules own real invariants. Lookup and CRUD modules do not.
- **Decision:** CRUD stays controller, service, and DTO. Modules listed in [name them] use presentation, application, domain, and infrastructure.
- **Consequences:** Do not generate a hexagonal skeleton for a new lookup resource. CQRS and event sourcing are not approved.

## ADR 4: Public API and vendor ports
- **Status:** Approved
- **Decision:** External HTTP lives under `/v1` with OpenAPI checked in CI. Payment, email, and storage are infrastructure adapters behind ports.
- **Consequences:** Breaking HTTP changes ship as `/v2` or a deprecated field with a removal date. Domain types do not include vendor enums.
