# Architectural Decision Records (ADR)

## ADR 1: Technical Stack Foundation
- **Status:** Approved
- **Context:** A small team needs one HTTP API with strong typing and a clear module boundary.
- **Decision:** Selected NestJS and TypeScript as a single process. Feature modules collaborate in-process. `@nestjs/microservices` is out of scope.
- **Consequences:** Team must enforce compilation and type checks at build time.

## ADR 2: Data Persistence
- **Status:** Approved
- **Context:** Relational data with transactional consistency is required.
- **Decision:** Selected PostgreSQL in every environment, changed only through committed migrations. Integrity is enforced with constraints.
- **Consequences:** Do not use SQLite locally. Do not edit production schema by hand.

## ADR 3: Thin controllers, no CQRS stack
- **Status:** Approved
- **Context:** CRUD and a few rules do not need a command bus.
- **Decision:** Controllers and pipes own HTTP. Use cases are service methods. Command, handler, validator, and mapper stacks are not the house style. Tactical DDD folders are not used.
- **Consequences:** A later complex module may supersede this ADR for that module only.

## ADR 4: One validation style and one error envelope
- **Status:** Approved
- **Decision:** Global `ValidationPipe` and one exception filter produce the client error shape.
- **Consequences:** New endpoints use DTOs. Hand-rolled body checks are non-compliant.
