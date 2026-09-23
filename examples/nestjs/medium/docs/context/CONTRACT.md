# Environment, External Systems & API Contracts

## Environment Templates
```env
# DO NOT PASTE REAL SECRETS HERE. USE TEMPLATE KEYS ONLY.
DATABASE_URL=postgresql://user:pass@localhost:5432/db
API_SECRET_KEY=your_placeholder_secret_key
LOG_LEVEL=info
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318
PORT=3000
```

## System Versions
- Runtime Version: Node.js 22.x
- Framework: NestJS with TypeScript `strict`
- Database Target: PostgreSQL 16
- Validation: class-validator and class-transformer
- Deployed secrets come from a secret manager. This file lists names only.

## Multi-Repo / API Gateways
- One API. No gateway and no per-module service.
- Public base path: `/v1`
- External API 1 Endpoint: `https://api.example.com/v1`
- Error body: `{ "error": { "code": "ORDER_ALREADY_PAID", "message": "This order has already been paid.", "traceId": "..." } }`
- OpenAPI is generated and diffed in CI. Removals go through deprecation.
- Payments and webhooks require `Idempotency-Key`.
