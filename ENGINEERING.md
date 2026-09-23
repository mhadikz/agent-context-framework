# Engineering Standard

This is the source of truth for how software in this framework is designed, built, and operated. The files under `templates/` and `examples/` are the applied form of this document. When an example and this document disagree, this document wins, and the example should be updated.

Every production application meets the baseline below. Advanced architecture is added when a named problem requires it. Company size alone is not that problem.

```text
Baseline          required for any production system
Growth            add when more than one team, a real domain, or production traffic appears
Justified         add only after an ADR names the problem it solves
```

## 1. Architecture and design

Separate responsibilities and keep a direction of dependencies.

```text
Presentation / API
        ↓
Application
        ↓
Domain
        ↑
Infrastructure
```

- Presentation maps a request or a screen onto a use case. It validates input, authenticates, and renders a result.
- Application orchestrates one use case: authorization, transactions, calls to ports.
- Domain holds business rules and invariants. It does not import a web framework, a database client, or a vendor SDK.
- Infrastructure implements ports: SQL, caches, email, payments, object storage.

Small programs may collapse application and domain into one module. The dependency direction still holds: UI and controllers do not own the rules, and the rules do not know which database is running.

Apply separation of concerns, high cohesion, low coupling, dependency inversion, and SOLID at module boundaries. Prefer composition. Share code when the concept is actually the same. A shared helper that every team must absorb breaking changes from is coupling, not DRY.

KISS and YAGNI govern new structure. A folder, package, service, or pattern is added when the current shape is failing, and the failure is written down.

Clean, hexagonal, and onion layouts are a way to enforce the direction above. Use them in modules that own real policy. A CRUD screen does not need four directories.

Each module has an owner. Its public surface is small. Other modules use that surface.

## 2. Domain-Driven Design

Use tactical DDD when the business rules are hard to state and easy to break. Skip it for administration screens that store and edit records.

When you use it:

- The code uses the language of the business.
- A bounded context owns a model. The same word may mean different things in two contexts, and that is acceptable.
- Entities have identity. Value objects carry small rules (`Money`, `DateRange`).
- Aggregates protect invariants through operations (`order.place()`), not through a 2,500-line service that reaches into every table.
- Domain services hold rules that fit no single entity.
- Domain events are facts in the past tense (`OrderPlaced`).
- Repositories persist aggregates. Application services are the use cases callers invoke.

A context map records which context publishes a contract and which contexts consume it.

## 3. Code quality

Code is read far more often than it is written. Optimize for the next reader.

- Names describe the domain concept.
- Functions and modules do one job. Split a file when it is no longer readable. A line limit is a signal, not a goal.
- Behavior is explicit. Clever code needs a simpler rewrite.
- Formatting and lint are automatic, so review time goes to behavior.
- The language type checker is on, in strict mode where the language has one. `any` and equivalent escapes are isolated at a boundary and explained.
- Warnings that the team ignores are deleted or turned into errors.
- Dead code and expired feature flags are removed on a schedule.
- Magic numbers and environment-specific values live in configuration.
- Prefer immutable values. Hidden global mutation is a defect.
- Side effects sit at the edges: the use case, the repository, the adapter.

## 4. Error handling

Classify failures and handle each class on purpose.

| Class | Meaning | Typical client result |
| --- | --- | --- |
| Validation | Input is malformed | 400 with field detail |
| Business | A rule refused the operation | 409 or 422 with a stable code |
| Auth | Missing identity or permission | 401 or 403 |
| Dependency | A downstream system failed | 502 or 503, or a graceful fallback |
| Unexpected | A bug or an unknown fault | 500 with a generic message, full detail in logs |

Catch at the boundary that can do something useful. An empty `catch` is a defect. Preserve the cause inside the process. Log unexpected failures with a correlation id. The client receives a stable envelope:

```json
{
  "error": {
    "code": "ORDER_ALREADY_PAID",
    "message": "This order has already been paid.",
    "traceId": "..."
  }
}
```

Stack traces, SQL, and policy-engine internals stay off the wire.

For calls that leave the process, set a timeout. Retry only when the operation is safe to run twice, with exponential backoff and jitter, and only at the layer that owns the call. Three layers that each retry three times become twenty-seven attempts during an outage.

## 5. Testing

Test behavior that would hurt if it broke. Coverage is a clue, not a target.

```text
              /\
             /  \
            / E2E\
           /------\
          /Contract\
         /----------\
        /Integration \
       /--------------\
      /      Unit      \
     --------------------
```

- Unit tests cover business rules, edge cases, and authorization decisions.
- Integration tests cover the database, migrations, and adapters, against the same engine production uses.
- Contract tests cover any API or event another team already consumes.
- End-to-end tests cover the few journeys that would page someone.
- A fixed production bug gets a regression test.

Include failure paths. A suite that only asserts the happy path is incomplete.

## 6. Security

Security is part of the design. The practical baseline comes from OWASP.

- Authenticate every protected entry point.
- Authorize every protected operation. See the next section.
- Validate input at the edge. Encode output. Keep parameterized queries as the only way SQL is built.
- Use the platform's CSRF strategy for cookie sessions. Set security headers, including a content security policy on anything a browser renders.
- Rate-limit authentication and public writes.
- Hash passwords with a dedicated algorithm (argon2 or bcrypt). Do not invent cryptography.
- Encrypt in transit. Encrypt sensitive data at rest when the threat model requires it.
- Scan dependencies in CI. Review new dependencies for license and weight.
- Audit sensitive operations. Rotate secrets.
- Least privilege on database roles, cloud credentials, and service accounts. The running app cannot drop tables.

Secrets never live in source, git history, Dockerfiles, client bundles, or committed config. They come from the environment or a secret manager.

## 7. Authentication is not authorization

A logged-in caller is not automatically allowed to perform the operation. Every protected use case answers both questions:

```text
Who are you?
Are you allowed to do this, to this resource?
```

Use the smallest model that fits:

- Resource ownership for "this user, this row".
- RBAC for stable roles.
- ABAC or a policy engine when the decision depends on attributes.
- Both, when roles and ownership both matter.

Hiding a button is not authorization. A middleware redirect is not authorization. The check runs in the use case that changes or reveals the data.

## 8. API design

A public API is a contract.

- Names and status codes are consistent. `201` creates, `401` means unauthenticated, `403` means unauthorized, `404` means missing, `409` means a conflict the client can understand.
- Lists are paginated, filtered, and sorted with a stable order. Unbounded result sets are a defect.
- The error envelope above is global.
- Every request has a correlation id, generated at the edge when the caller did not send one.
- Write operations that must not apply twice accept an idempotency key.
- Document the contract (OpenAPI or an equivalent schema) and diff it in CI once another team depends on it.
- Prefer additive changes. Removing or renaming a field that someone else reads requires a version or a deprecation window.

## 9. Idempotency

Payments, order creation, webhooks, and job handlers can be delivered twice. Design for that.

- The client sends `Idempotency-Key`.
- The server stores the key with the outcome, uniquely.
- A repeat returns the original outcome and does not charge, ship, or email again.
- Consumers of queues treat delivery as at least once.

## 10. Database engineering

The schema is part of the design, not a side effect of the ORM.

- Primary keys, foreign keys where the relationship is real, check constraints, unique constraints, and indexes for the queries you actually run.
- Correct types. Money is not a binary float. Timestamps are stored in UTC.
- Transactions around writes that must succeed or fail together.
- The database enforces critical integrity. A prior `SELECT` then `INSERT` loses under concurrency. `UNIQUE(email)` does not.
- A backup exists before production, and a restore has been rehearsed.
- Connection pools are bounded.

Optimistic locking (`version`) protects rows that two requests can edit, such as balances and inventory. Pessimistic locks are for short critical sections, not for request-long transactions.

## 11. Database migrations

Schema changes are versioned, reviewed, and applied by the pipeline. Nobody edits production by hand.

While two versions of the app can run at once, change the schema in three steps:

```text
Expand     add the new shape; old code still works
Migrate    backfill; switch readers and writers
Contract   remove the old shape in a later release
```

A migration that rewrites a hot table needs a plan in the pull request: lock time, batching, and how to stop.

## 12. Configuration

Code is constant. Configuration changes per environment.

```text
DATABASE_URL
LOG_LEVEL
CACHE_TTL
PAYMENT_PROVIDER
FEATURE_X_ENABLED
```

Validate configuration when the process boots. Invalid configuration fails startup.

Behavior does not fork through scattered `if (environment == "production")` checks. The difference between environments is the value of the configuration, not a second copy of the code.

Client-shipped configuration is public. A `NEXT_PUBLIC_` variable, a mobile bundle, and a browser script are not secret stores.

## 13. Logging

Log structured events.

```json
{
  "level": "error",
  "event": "payment_failed",
  "orderId": "123",
  "provider": "stripe",
  "traceId": "abc"
}
```

Logs include the correlation id and the ids needed to debug. They omit passwords, tokens, API keys, session cookies, and raw payment or government identifiers. Personal data in a log is a product decision, not a debugging convenience.

## 14. Observability

Logs are one signal. Production systems also have metrics and traces.

Watch, at minimum:

```text
request rate
error rate
latency
saturation of CPU, memory, and database connections
queue depth, when a queue exists
failed calls to external providers
```

Distributed traces matter as soon as a request leaves the process. Alerts fire on symptoms users feel, not only on CPU.

## 15. Health checks

Expose two answers:

```text
/health/live    Is the process alive?
/health/ready   Can it accept traffic right now?
```

Liveness failures restart the process. Readiness failures stop new traffic. Readiness checks the dependencies the process cannot serve without, and fails closed when they are down. A check that always returns 200 is not a health check.

## 16. CI/CD

A production deploy is built by the pipeline, from a known commit, not from a laptop.

```text
Commit
  → format and lint
  → typecheck
  → unit tests
  → integration tests
  → dependency and secret scan
  → build an immutable artifact
  → deploy
  → smoke test
```

The artifact that was tested is the artifact that is deployed. Dependencies are locked.

## 17. Code review

Review correctness, architecture, security, failure behavior, and tests. Formatting is the linter's job.

The author keeps the change small enough to review. The reviewer looks for missing authorization, missing constraints, and compatibility breaks before style.

## 18. Git

- Small commits with messages that say why.
- Short-lived branches into a protected default branch.
- Required CI before merge.
- No secrets and no generated junk.
- Tags or releases for anything you may need to roll back to.

Trunk-based development fits this standard: `main` plus small branches. Long-lived feature branches hide integration risk.

## 19. Dependency management

Every dependency is a maintenance and security cost.

- Lock the tree.
- Scan for known vulnerabilities in CI.
- Review additions: weight, license, bus factor, and whether the standard library already does the job.
- Update on a cadence. Pin production base images and actions to a version, not `latest`.
- A one-line helper does not justify a large library.

## 20. Documentation

The repository README answers:

```text
What is this?
How do I run it?
How do I test it?
How do I configure it?
How do I deploy it?
Where is the architecture?
```

Larger systems also keep a context map, API docs, operational runbooks, and the ADRs below. A diagram that nobody updates is worse than a short ADR.

## 21. Architecture Decision Records

Record decisions a later team could reasonably undo.

```text
Status
Context
Decision
Alternatives
Consequences
```

ADRs are short. They are the memory of why PostgreSQL, why a modular monolith, why a vendor port exists. A new service, a new broker, CQRS, and event sourcing each require one before the code lands.

## 22. Feature flags

Deploy and release are different events.

```text
Deploy the code
  → enable internally
  → enable for a small share
  → enable for everyone
```

A flag has an owner and an expiry. After rollout, delete the flag and the dead branch. A permanent `if (flag)` is unfinished work.

## 23. Performance

Measure, find the bottleneck, change it, measure again. Do not optimize from a guess.

Look first at N+1 queries, missing indexes, chatty downstream calls, unbounded payloads, accidental serialization, and large client bundles. Add a cache only after a measurement says the database or the origin is the cost, and write down invalidation before writing the cache.

## 24. Caching

For every cache, name:

```text
What is stored?
How long does it live?
How is it invalidated?
What happens when it is stale?
Who is allowed to see it?
```

Personalized responses are not stored as public CDN content. The database remains the source of truth. Redis, the CDN, the framework cache, and the browser cache are different tools with different failure modes.

## 25. Concurrency

Two requests can update the same row. Assume they will.

Use transactions, unique constraints, atomic updates, and optimistic or pessimistic locking as the case requires. Distributed locks are a last resort and need a timeout and a story for the lock holder dying.

"Request A finishes before request B" is not a design.

## 26. Messaging

Once a message crosses a process, assume duplicates, delays, and reordering.

```text
Producer → outbox in the same transaction as the state change → broker → consumer
```

The consumer is idempotent. The broker has a dead-letter path. Poison messages are quarantined with the reason. Ordering, if required, is per entity key and is stated. Schema changes on events are additive and versioned.

A dual write to the database and the broker without an outbox will lose or double-publish events.

## 27. Graceful shutdown

On `SIGTERM`:

```text
Stop accepting new work
  → finish in-flight requests and jobs
  → close database pools and clients
  → exit
```

This is required in containers and in any platform that replaces processes during deploy.

## 28. Reliability patterns

Use these at process boundaries, and only where a real failure mode exists:

```text
Timeout
Retry with exponential backoff and jitter
Circuit breaker
Bulkhead
Rate limit
Fallback
Dead-letter queue
```

A circuit breaker around a healthy local function adds failure. A retry on a non-idempotent payment creates a second charge. Bulkheads stop one vendor from exhausting the process's connections.

## 29. External integrations

Treat every external system as unreliable. It can time out, return garbage, return 500, rate-limit you, or change a field.

Hide it behind a port:

```text
PaymentGateway
EmailSender
ObjectStorage
```

The domain sees your types, not the vendor's. Verify webhook signatures before the handler runs. Timeouts are mandatory.

## 30. Backward compatibility

While a deploy is rolling, old and new code run together. During that window:

- API responses only add fields.
- Event payloads only add fields.
- The database still matches the previous release.
- Configuration the old process reads is still present.

Breaking changes are a new version, or a deprecation with a date.

## 31. Deployment safety

Choose a strategy and write down three answers before production:

```text
How do we detect a bad release?
How do we roll back?
How do we recover data the bad release wrote?
```

Rolling, blue/green, and canary deploys are all valid. All of them require the compatibility rules above. A rollback that cannot run against the new schema is not a rollback.

## 32. Infrastructure as code

Permanent infrastructure is reviewed code: Terraform, OpenTofu, Pulumi, CloudFormation, or the platform's equivalent. Clicking a cloud console for a lasting resource is drift.

Application teams consume the platform. They do not fork a second way to create databases and networks.

## 33. Containers

When you ship a container:

- Pin the base image. `latest` is not a version.
- Run as a non-root user.
- Do not bake secrets into the image.
- Set a health check and resource limits.
- Handle `SIGTERM`.
- Scan the image in CI.

The image is immutable. Configuration arrives at runtime.

## 34. Environment parity

Local, CI, and production use the same class of backing services. PostgreSQL in production means PostgreSQL in tests. A SQLite-only laptop hides constraint, type, and concurrency bugs.

Auth mode and TLS differences that matter in production are either reproduced or explicitly tested.

## 35. Threat modeling

Before building a flow that touches money, personal data, admin power, or a new trust boundary, write a short note:

```text
What are we protecting?
Who would abuse it?
How would they do it?
What is the impact?
What mitigation lands with this change?
```

The note can be a section of the pull request. It exists before the code is called done.

## 36. Data privacy

For every new piece of personal data:

```text
What is it?
Why do we store it?
Who can read it?
How long do we keep it?
How do we delete it?
Where does it sit?
```

Collect the minimum. Retention is a column decision, not a later policy project. Deletion has to be possible, including in backups within the retention window you publish.

## 37. Auditability

For money movement, permission changes, admin writes, and security events, record:

```text
who
did what
to what
when
from where
```

The audit stream is separate from debug logs and is harder to alter. Debug logs are not an audit log.

## 38. Business invariants

A rule that must always be true lives in one place, the domain, and is backed by the database when the database can express it.

```text
An order cannot ship before payment.
A withdrawal cannot exceed the available balance.
A booking cannot end before it starts.
```

The UI, the controller, the cron job, and the webhook all call that place. A copy of the rule in a second layer will drift.

## 39. Modular design

A modular monolith is the default shape for a product with several capabilities:

```text
Application
├── Identity
├── Customers
├── Orders
├── Payments
├── Inventory
└── Notifications
```

Modules communicate through facades or in-process events with a visible subscriber list. They do not read each other's tables. This gives team ownership without distributed-system failure modes.

## 40. Microservices

A network service is a new system: partial failure, discovery, tracing, versioning, and the loss of a shared transaction.

Extract a module only when an ADR names one of these:

- It must scale or fail independently of the rest.
- A compliance boundary forbids a shared database.
- A team cannot meet its release needs inside the monolith.

After extraction, that service has its own database. Shared tables across services are forbidden. Cross-service workflows are sagas or process managers, not distributed transactions.

## 41. CQRS

Separate the read model from the write model when they are actually different: search, a dashboard, a feed that is not the aggregate.

A stack of `CreateUserCommand`, handler, validator, response, and mapper around a single insert is ceremony. Ordinary CRUD is a use case and a table.

## 42. Event sourcing

Store events as the source of truth only when the history of changes is the product: a ledger, trading, or a regulated workflow where "what happened" matters more than "what is true now".

It costs more than a table: projections, replay, schema evolution, and harder debugging. Looking sophisticated is not a requirement.

## 43. Twelve-factor baseline

For a service that runs in the cloud:

- One codebase per app, many deploys.
- Dependencies declared and isolated.
- Configuration in the environment.
- Backing services attached as resources.
- A separated build, release, and run.
- Stateless processes. Sessions and files live in backing services.
- Port binding.
- Concurrency by processes.
- Fast startup and graceful shutdown.
- Dev/prod parity.
- Logs as event streams.
- Admin tasks as one-off processes, using the same code.

## 44. Operational readiness

A change is production-ready when the owning team can answer yes:

```text
Can we deploy it?
Can we monitor it?
Can we debug it?
Can we roll it back?
Can we recover the data?
Can we scale it if we must?
Can we secure it?
Can someone else maintain it?
```

Code that has merged without those answers is unfinished.

---

## 45. Additions beyond the original checklist

These are part of this standard because the original list did not name them, and later example work needs them.

### Time, money, and identifiers

- Store instants in UTC. Convert to a zone at the edge.
- Represent money as an integer in minor units plus a currency, or as a decimal type the database supports. Never as a binary float.
- Identifiers that leave the process are unguessable where they grant access. Sequential ids in a public URL are a choice you write down.
- Clocks skew. Do not depend on two machines sharing a millisecond.

### Accessibility

Anything a person uses through a browser meets an accessibility bar: named controls, visible focus, contrast, keyboard use, and text that is not only color. The design system owns the primitives. A feature does not invent a second focus ring.

### Supply chain

- Lockfiles are committed.
- CI rejects known vulnerable dependencies above the agreed severity.
- Secret scanning runs on commits.
- Build actions and base images are pinned.
- A generated SBOM is produced for any artifact you ship to others, once you have external consumers.

### Service levels

A critical journey has an SLO: availability and latency, with an error budget. Alerts page on budget burn and on user symptoms. A dashboard nobody watches is not an SLO.

### Webhooks and inbound events

- Verify the signature before any state change.
- Store the provider's event id uniquely.
- Ack only after the work is durable, or ack and process from a queue with a retry. Do not ack and then forget.
- Respond within the provider's time limit by doing the slow work asynchronously.

### Data lifecycle

- Backups are automated. Restores are drilled.
- Migrations are forward-fixable when they are not reversible.
- Soft delete is a product decision with a retention clock, not the default for every table.
- Test data and production data do not share a database.

### Contract tests

When another team or another process consumes your HTTP or your events, a contract test fails the build if you break them. The consumer's expectations live in the repository that publishes the contract, or in a pact the consumer owns. Hand-copied types in the consumer are a drift bug.

### Change control for automated assistants

An assistant follows this document and the ADRs. It does not add a service, a broker, a new framework, or a dependency because the pattern is popular. It does not weaken auth, log personal data, or commit a secret to get a test passing. A structural change lands as an ADR first.

---

## What each team size takes from this document

Use this map when writing or revising an example. "Baseline" means the section applies in full. "Light" means the rule applies in a reduced form. "When needed" means the section stays out until the trigger is true.

| Topic | Small (about 1–8 engineers) | Medium (several squads) | Large / enterprise |
| --- | --- | --- | --- |
| Boundaries and ownership | Feature or module folders | Facades between modules | Bounded contexts and a context map |
| Layered architecture | Collapsed, direction still holds | Layers inside complex modules | Layers in every context that owns policy |
| Tactical DDD | When one feature's rules outgrow a module | In the complex contexts only | In contexts that own invariants |
| Code quality, types, lint | Baseline | Baseline | Baseline, warnings are errors |
| Errors and the envelope | Baseline | Baseline, domain error types | Baseline, catalogued codes |
| Tests | Unit + a few integration + critical e2e | Add contract tests for shared routes | Contract tests gate consumers |
| Security baseline | Baseline | Plus threat notes on risky flows | Plus scanning, rotation, ASVS-level review |
| Authn and authz | Ownership or simple roles | Roles plus ownership, tested | Platform policy, RBAC and ABAC as required |
| API shape | Envelope, pagination | Version when a second consumer exists | Versioned, additive, diffed in CI |
| Idempotency | Payments and webhooks | Same, stored uniquely | Same, plus consumer idempotency |
| Database constraints | Baseline | Plus optimistic locking where needed | Baseline, per-context ownership of tables |
| Migrations | Committed, same engine everywhere | Expand, migrate, contract | Same, plus a restore drill |
| Configuration | Validated env, no secrets in client | Same, secret manager in deploys | Same, rotation |
| Logging | Structured, no secrets | Plus correlation id | Plus platform pipeline |
| Metrics and traces | Error tracking | Logs, metrics, traces | SLOs on critical journeys |
| Health and shutdown | Ready check and SIGTERM | Same for API and worker | Platform probes, fail closed |
| CI | Lint, types, unit, build | Plus integration and a scan | Plus SAST, image scan, smoke, artifact |
| Review and git | Small PRs, protected branch | CODEOWNERS per module | Owners on contexts and contracts |
| Dependencies | Locked, ask before adding | Audit in CI | License policy and pinned images |
| Docs | README | Plus ADRs and a runbook | Plus context map and on-call docs |
| Feature flags | Later | For risky releases, then delete | Default off, owner, expiry |
| Performance | Measure before caching | Index and query review | Budgets, including client bundles |
| Caching | Only after a measurement | TTL and invalidation written down | No public cache of personalized data |
| Concurrency | Constraints and transactions | Optimistic locking | Same, stated across services |
| Messaging | In process, or the database as a queue | Same until a broker is justified | Outbox, DLQ, versioned events |
| External ports | A function boundary is enough | Named ports and fakes in tests | Anti-corruption layers |
| Compatibility | Avoid breaking your own UI | Additive once others call you | Required during every rolling deploy |
| Deploy and rollback | CI deploy, a way back | Staging, smoke, rollback note | Canary or blue/green, rehearsed |
| Infrastructure as code | Platform defaults | The few resources you own | All permanent infrastructure |
| Containers | If you use them, the container rules | Same | Same, resource limits |
| Environment parity | Same database engine | Same | Same, including auth mode where it matters |
| Threat modeling | On the first money or personal-data flow | On each new risky flow | On each new trust boundary |
| Privacy | Minimum data, a retention note | Same | Inventory, deletion path |
| Audit | Admin and money actions | Same, queryable | Separate, tamper-resistant stream |
| Invariants | One module | One domain model | One context |
| Microservices | Out of scope | ADR required | ADR required, own database |
| CQRS and event sourcing | Out of scope | ADR required | ADR required |
| Twelve-factor | Config, stateless, logs | Baseline | Baseline |
| Operational readiness | Deploy, debug, roll back | Plus monitor and recover | The full list |
| Accessibility | If there is a UI | Design-system primitives | Design system is mandatory |
| Supply chain | Lockfile | Vulnerability scan | Secret scan, pinned images, SBOM if you ship artifacts |
| SLOs | Later | On the main journey | Error budgets and paging |
| Webhooks | Signature and idempotency | Same | Same, plus a DLQ |
| Assistant change control | Follow the ADRs | Same | Same |

## Production-ready checklist

A release candidate fails if any baseline item that applies to its team size is missing.

**Design**

- [ ] Business rules live in one place, not in the UI and the controller and a job.
- [ ] Modules have owners and a small public surface.
- [ ] Advanced patterns in use have an ADR.

**Behavior**

- [ ] Input is validated at the edge.
- [ ] Protected operations check identity and permission.
- [ ] Errors use the standard envelope and do not leak internals.
- [ ] Unsafe creates are idempotent.
- [ ] Invariants the database can express are constraints.

**Change**

- [ ] Migrations are automated and compatible with the previous release.
- [ ] Configuration is outside the code and validated at boot.
- [ ] Dependencies are locked and scanned.
- [ ] CI builds the artifact that production runs.

**Operations**

- [ ] Logs are structured and free of secrets.
- [ ] There is a readiness signal and a graceful shutdown path.
- [ ] There is a way to detect a bad deploy and roll it back.
- [ ] Backups exist, and someone has restored one.
- [ ] Another engineer can run, test, and deploy from the README.

**Only when the team-size map says so**

- [ ] Traces cross process boundaries.
- [ ] Public contracts are versioned and tested.
- [ ] Sensitive actions are audited.
- [ ] Personal data has a retention and deletion story.
- [ ] Feature flags in the release have owners and expiry dates.
