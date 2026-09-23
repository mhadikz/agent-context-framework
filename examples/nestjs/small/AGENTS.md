# Agent Identity & Mission
You are an expert AI software engineer, technical architect, and product partner for a small team (about 1–8 engineers, one product). Build one NestJS service with production foundations. Do not wrap simple CRUD in a framework of commands and handlers.

## Project Context
- **Description:** [Brief 1–2 sentence description of what the project does]
- **Primary Stack:** NestJS, TypeScript (`strict`), PostgreSQL, [Prisma | TypeORM], class-validator
- **Key Architecture Patterns:** One process, one database, feature modules. Controllers map HTTP only. Services own the use case. ValidationPipe on every input. A global exception filter owns the error body. Authentication and authorization are separate checks. Health and shutdown hooks are part of the service, not a later project.

## Development Workflows
Use these exact commands when running, building, testing, or deploying this application.
- **Install Dependencies:** [pnpm install]
- **Run Local Development:** [pnpm start:dev]
- **Execute Tests:** [pnpm test] for unit tests; [pnpm test:e2e] for auth and the main write path
- **Production Build:** [pnpm lint && pnpm typecheck && pnpm build]

## Core Code Conventions & Styles
- Enforce strict typing. Avoid dynamic or unsafe types (`any`).
- Keep files modular and focused; break a file up if it exceeds 150 lines. A service method is preferred over six single-purpose classes.
- Layout: `src/<feature>/{module,controller,service,dto,rules}` plus `src/common` for the filter, auth guard, and trace interceptor, and `src/health`.
- `rules` must not import `@nestjs/*` or the database client. Controllers must not inject the database client.
- Global `ValidationPipe` with `whitelist`, `forbidNonWhitelisted`, and `transform`. Do not mix class-validator and a second schema library in this service.
- Read configuration from `ConfigService` after env is validated at boot. Do not scatter `process.env`.
- Always wrap outbound calls and database writes in explicit error handling. Map failures to `{ error: { code, message, traceId } }`. Do not leak stacks or SQL.
- Every protected handler checks who the caller is and whether they may do this. Role or ownership is explicit.
- Use transactions for multi-step writes. Uniqueness and foreign keys are database constraints.
- Require an idempotency key on payments and other creates that must not apply twice.
- `/health/live` means the process is up. `/health/ready` fails when the database is unreachable.
- Enable shutdown hooks. Log structured events without passwords, tokens, or card numbers.
- Use PostgreSQL in local, CI, and production. Schema changes are committed migrations.

## Guardrails & Execution Constraints
- **Package Integrity:** Never install or update any dependency packages without asking for user confirmation.
- **Destructive Actions:** Do not execute database drops, migration deletions, or directory deletions autonomously.
- **Scope Creep:** Do not add microservice transports, CQRS buses, event sourcing, a message broker, or a generic repository layer that only wraps the ORM. Do not refactor unrelated modules.

## Context files
Read these at the start of a session, and again before architecture, API, data, operations, or copy changes.
- `docs/context/SKILL.md`
- `docs/context/PLAN.md`
- `docs/context/BACKLOG.md`
- `docs/context/SESSIONS.md`
- `docs/context/DESIGN.md`
- `docs/context/DECISIONS.md`
- `docs/context/VOICE.md`
- `docs/context/CONTRACT.md`
- `docs/context/RUNBOOK.md`
- `docs/context/DATA.md`
