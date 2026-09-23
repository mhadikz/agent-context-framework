# Operational Runbook

## Deploy
- The pipeline runs lint, typecheck, unit tests, and `pnpm build`, then deploys that artifact.
- Do not deploy from a laptop.

## Detect a bad release
- Watch the error tracker and the readiness check.
- Smoke the sign-in path and the primary write after deploy.

## Roll back
- Redeploy the previous artifact.
- Do not ship a migration that the previous artifact cannot run.

## Recover data
- Backup location: [managed database backups]
- Last restore drill: [date or "not yet"]

## Failed migration
- Stop the deploy. Add a forward-fix migration. Do not edit production by hand.

## Stuck webhook
- Find the delivery by `traceId` and idempotency key.
- Confirm the signature check failed or the handler threw.
- Replay only after the key is safe to run again.
