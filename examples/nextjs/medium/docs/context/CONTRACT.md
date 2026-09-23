# Environment, External Systems & API Contracts

## Environment Templates
```env
# DO NOT PASTE REAL SECRETS HERE. USE TEMPLATE KEYS ONLY.
DATABASE_URL=postgresql://user:pass@localhost:5432/db
AUTH_SECRET=your_placeholder_secret_key
LOG_LEVEL=info
# Optional. Add only after a cache ADR. Do not point production at a laptop Redis.
REDIS_URL=redis://localhost:6379
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## System Versions
- Runtime Version: Node.js 22.x
- Framework: Next.js (App Router) with TypeScript `strict`
- Database Target: PostgreSQL 16
- Validation: [Zod], shared when more than one app calls the use case
- Deployed secrets come from a secret manager. Names here are templates only.

## Multi-Repo / API Gateways
- Default is one web app. External consumers use Route Handlers under a documented contract, not a second service.
- External API 1 Endpoint: `https://api.example.com/v1`
- Error body: `{ "error": { "code": "ORDER_ALREADY_PAID", "message": "This order has already been paid.", "traceId": "..." } }`
- List endpoints are paginated. Breaking response changes need a version or a deprecation window.
- Payments and webhooks require `Idempotency-Key`.
