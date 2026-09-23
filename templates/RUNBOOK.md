# Operational Runbook

## Deploy
- [How a release is built and shipped. Deploys come from CI, not from a laptop.]

## Detect a bad release
- [What you watch: error rate, latency, failed health check, smoke test.]

## Roll back
- [How to return to the previous artifact.]
- [Whether the new schema still allows the previous version to run.]

## Recover data
- Backup location: [where]
- Last restore drill: [date or "not yet"]

## Failed migration
- Stop the deploy. Fix forward with a new migration. Do not edit the production database by hand.

## Stuck async work
- [Webhook, job, or consumer: where to look, and how to replay safely.]
