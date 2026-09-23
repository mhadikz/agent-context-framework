# Operational Runbook

## Deploy
- CI runs lint, types, unit tests, integration tests, and the production build, then deploys to staging and smoke-tests before production.
- Do not deploy from a laptop.

## Detect a bad release
- Watch error rate, latency, and failed jobs.
- Traces for the release include `traceId`.

## Roll back
- Redeploy the previous web artifact.
- The schema still matches that artifact (expand, migrate, contract).
- A risky change has a feature flag that can be turned off without a deploy.

## Recover data
- Backup location: [managed backups]
- Last restore drill: [date or "not yet"]

## Failed migration
- Stop after the expand step if backfill fails. Do not contract in the same release.

## Stuck webhook or job
- The worker and the web app share the use case. Look up the idempotency key, then replay the use case. Do not write a second copy of the SQL.
