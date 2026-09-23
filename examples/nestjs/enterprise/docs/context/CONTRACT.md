# Environment, External Systems & API Contracts

## Environment Templates
```env
# DO NOT PASTE REAL SECRETS HERE. USE TEMPLATE KEYS ONLY.
DATABASE_URL=postgresql://user:pass@localhost:5432/db
API_SECRET_KEY=your_placeholder_secret_key
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318
LOG_LEVEL=info
PORT=3000
```

## System Versions
- Runtime Version: Node.js 22.x
- Framework: NestJS with TypeScript `strict`
- Database Target: PostgreSQL 16
- Telemetry: OpenTelemetry
- Workload identity and the secret manager supply deployed credentials. Rotate them. Do not copy production values here.

## Multi-Repo / API Gateways
- External clients enter through [the API gateway]. Each context still authorizes.
- Public HTTP is versioned (`/v1`). Schemas change additively.
- External API 1 Endpoint: `https://api.example.com/v1`
- Error body: `{ "error": { "code": "ORDER_ALREADY_PAID", "message": "This order has already been paid.", "traceId": "..." } }`
- Integration events are versioned facts. Consumers tolerate duplicate delivery. A dead-letter path is required.
- Commands that move money or create obligations require `Idempotency-Key`.
