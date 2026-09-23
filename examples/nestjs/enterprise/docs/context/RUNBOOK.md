# Operational Runbook

## Deploy
- CI builds an immutable image, scans it, and ships by rolling or canary.
- `/health/live` and `/health/ready` are the probes. Readiness fails closed.
- Old and new processes run together, so schema and `/v1` stay compatible.

## Detect a bad release
- Page on the SLO for the critical journeys: error rate, latency, and queue age.
- Follow `traceId` from the gateway into the owning context.

## Roll back
- Return traffic to the previous image.
- Confirm that image still matches the schema.
- Disable the feature flag when it gates the bad behavior.

## Recover data
- Backup location: [per-context database backups]
- Last restore drill: [date]
- Rollback does not undo rows. A bad write needs a forward fix.

## Failed migration
- Remain on expand until backfill is verified. Contract ships later.

## Stuck consumer
- Confirm the event version and the idempotency key.
- Replay from the outbox. Poison messages sit on the dead-letter path with the reason.
- On SIGTERM, consumers stop intake, finish the current message, then close the pool.
