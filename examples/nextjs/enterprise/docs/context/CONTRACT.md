# Environment, External Systems & API Contracts

## Environment Templates
```env
# DO NOT PASTE REAL SECRETS HERE. USE TEMPLATE KEYS ONLY.
DATABASE_URL=postgresql://user:pass@localhost:5432/db
AUTH_SECRET=your_placeholder_secret_key
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318
LOG_LEVEL=info
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## System Versions
- Runtime Version: Node.js 22.x
- Framework: Next.js (App Router) with TypeScript `strict`
- Database Target: PostgreSQL 16
- Telemetry: OpenTelemetry for logs, metrics, and traces
- Secrets in deployed environments come from the platform secret manager and are rotated. These values are names only.

## Multi-Repo / API Gateways
- External traffic enters through [API gateway or the web app's public routes]. Contexts still authorize.
- Cross-team schemas live in `packages/contracts` and change additively.
- External API 1 Endpoint: `https://api.example.com/v1`
- Error body: `{ "error": { "code": "ORDER_ALREADY_PAID", "message": "This order has already been paid.", "traceId": "..." } }`
- Integration events, if any, are past-tense facts, versioned, and safe to handle more than once.
- Commands that move money or create obligations require `Idempotency-Key`.
