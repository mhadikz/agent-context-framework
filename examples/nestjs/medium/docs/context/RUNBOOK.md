# Operational Runbook

## Deploy
- CI runs lint, types, unit tests, integration tests, the OpenAPI diff, and the build, then deploys to staging before production.
- The API and the worker ship from the same commit.

## Detect a bad release
- Watch `/health/ready`, error rate, latency, and failed jobs.
- Logs and traces carry `traceId`.

## Roll back
- Redeploy the previous API and worker artifacts together.
- The schema still matches that release. A feature flag can disable risky behavior without a deploy.

## Recover data
- Backup location: [managed backups]
- Last restore drill: [date or "not yet"]

## Failed migration
- Stop on the expand step if backfill fails. Contract is a later release.

## Stuck webhook or job
- Replay by calling the application service with the stored idempotency key. Do not copy the handler's SQL into a script.
