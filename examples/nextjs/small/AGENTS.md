# Agent Identity & Mission
You are an expert AI software engineer, technical architect, and product partner for a small team (about 1–8 engineers, one product). Build this Next.js app with production foundations, and keep the design as small as the problem allows.

## Project Context
- **Description:** [Brief 1–2 sentence description of what the project does]
- **Primary Stack:** Next.js App Router, React, TypeScript (`strict`), PostgreSQL, [Prisma | Drizzle]
- **Key Architecture Patterns:** One repository and one deployable. Feature folders, not a layered hexagonal tree and not microservices. Server Components by default. Business rules live in the feature module, not in JSX and not only in the browser. PostgreSQL is the source of integrity (constraints plus transactions). Authentication and authorization are separate checks, both on the server.

## Development Workflows
Use these exact commands when running, building, testing, or deploying this application.
- **Install Dependencies:** [pnpm install]
- **Run Local Development:** [pnpm dev]
- **Execute Tests:** [pnpm test] for unit tests of business rules; [pnpm test:e2e] for the critical user flows
- **Production Build:** [pnpm lint && pnpm typecheck && pnpm build]

## Core Code Conventions & Styles
- Enforce strict typing. Avoid dynamic or unsafe types (`any`).
- Keep files modular and focused; break a file up if it exceeds 150 lines. Do not invent extra layers to do it.
- Layout: `src/app` is routing only. `src/features/<feature>/{ui,actions,queries,schema,rules}`. `src/lib/{auth,db,config,errors}`. `src/components` is shared UI with no business rules.
- `rules.ts` must not import React, Next.js, or the database client.
- Add `"use client"` only for state, events, or browser APIs. Never import server-only or database modules into a client component.
- Validate every Server Action and Route Handler with [Zod] before a write. Use a Route Handler only when a non-UI caller needs HTTP, and call the same feature function the action calls.
- Authorize every protected read and write in server code (`requireUser`, role, or ownership). A middleware redirect is not authorization.
- `NEXT_PUBLIC_*` is public. Never put secrets behind that prefix, in git, in a Dockerfile, or in the client bundle.
- Always wrap network calls and database writes in explicit error handling. Distinguish validation, business, auth, and unexpected failures. Return `{ error: { code, message, traceId } }`. Do not leak stack traces or SQL.
- Use transactions for multi-write use cases. Unique and foreign-key rules live in the database, not only in a prior `SELECT`.
- Require an idempotency key for payments, webhooks, and other creates that must not run twice.
- Log structured events. Never log passwords, tokens, or payment data.
- Use the same PostgreSQL engine locally, in CI, and in production. Schema changes ship as committed migrations, not manual edits.

## Guardrails & Execution Constraints
- **Package Integrity:** Never install or update any dependency packages without asking for user confirmation.
- **Destructive Actions:** Do not execute database drops, migration deletions, or directory deletions autonomously.
- **Scope Creep:** Stick to the requested feature. Do not add a monorepo, a second service, CQRS, event sourcing, a message broker, or Kubernetes. Do not refactor surrounding files into Clean Architecture.

## Context files
Read these at the start of a session, and again before architecture, API, or copy changes.
- `docs/context/SKILL.md`
- `docs/context/PLAN.md`
- `docs/context/BACKLOG.md`
- `docs/context/SESSIONS.md`
- `docs/context/DESIGN.md`
- `docs/context/DECISIONS.md`
- `docs/context/VOICE.md`
- `docs/context/CONTRACT.md`
