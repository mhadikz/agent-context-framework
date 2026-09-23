# Architectural Decision Records (ADR)

## ADR 1: Technical Stack Foundation
- **Status:** Approved
- **Context:** A small team needs one deployable, strong typing, and a UI framework that can own the request path.
- **Decision:** Selected Next.js (App Router) and TypeScript over a separate API plus SPA. Server Components and Server Actions are the default. A Route Handler is added only for a non-UI caller.
- **Consequences:** Team must enforce compilation and type checks at build time. Client components stay presentational.

## ADR 2: Data Persistence
- **Status:** Approved
- **Context:** Relational data with transactional consistency is required.
- **Decision:** Selected PostgreSQL in local, CI, and production, with committed migrations. Critical uniqueness and references are database constraints.
- **Consequences:** Do not use a different database engine in development. Do not apply production schema changes by hand.

## ADR 3: Feature folders, not layered architecture
- **Status:** Approved
- **Context:** CRUD and a small set of rules do not justify hexagonal ceremony.
- **Decision:** Each feature owns `ui`, `actions`, `queries`, `schema`, and `rules`. Clean Architecture, tactical DDD, CQRS, and event sourcing are not used.
- **Consequences:** Introduce those patterns only by superseding this ADR when a feature's rules no longer fit one module.

## ADR 4: Authorization on the server
- **Status:** Approved
- **Context:** A logged-in user is not automatically allowed to perform an operation.
- **Decision:** Every protected query and mutation checks identity and permission in server code.
- **Consequences:** Middleware is a redirect convenience only. UI hiding is not an access-control mechanism.
