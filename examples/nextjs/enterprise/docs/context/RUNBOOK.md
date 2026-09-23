# Operational Runbook

## Deploy
- CI builds one immutable artifact, scans it, deploys by rolling or canary, then smokes the critical journeys.
- Old and new versions run together. Schema and contracts stay compatible for that window.

## Detect a bad release
- Page on the SLO for the primary journeys: error rate and latency, not only CPU.
- `traceId` connects the browser request to the owning context.

## Roll back
- Shift traffic back to the previous artifact.
- Confirm the previous artifact still matches the current schema.
- Turn off the feature flag before redeploying when the flag gates the bad behavior.

## Recover data
- Backup location: [platform backups for each context database]
- Last restore drill: [date]
- A bad release that wrote data needs a forward fix, not a hope that rollback undoes rows.

## Failed migration
- Stay on the expand step until backfill is verified. Contract is a later release.

## Stuck event or webhook
- Verify the signature or the contract version.
- Replay from the outbox or the provider id. Consumers are idempotent.
- Poison messages go to the dead-letter path with the reason.
