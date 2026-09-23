# Operational Runbook

## Deploy
- CI runs lint, typecheck, unit tests, and `pnpm build`, then deploys that artifact.
- Do not deploy from a laptop.

## Detect a bad release
- `/health/ready` fails when the database is unreachable.
- Watch the error tracker for unexpected 500s with a `traceId`.

## Roll back
- Redeploy the previous artifact.
- Do not ship a migration the previous process cannot run.

## Recover data
- Backup location: [managed database backups]
- Last restore drill: [date or "not yet"]

## Failed migration
- Stop the deploy. Add a forward-fix migration. Do not edit production by hand.

## Shutdown
- On SIGTERM the process stops accepting requests, finishes in-flight work, closes the pool, and exits.
