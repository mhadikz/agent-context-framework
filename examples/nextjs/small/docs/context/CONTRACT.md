# Environment, External Systems & API Contracts

## Environment Templates
```env
# DO NOT PASTE REAL SECRETS HERE. USE TEMPLATE KEYS ONLY.
DATABASE_URL=postgresql://user:pass@localhost:5432/db
AUTH_SECRET=your_placeholder_secret_key
# NEXT_PUBLIC_ values are shipped to the browser. Never put a secret here.
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## System Versions
- Runtime Version: Node.js 22.x
- Framework: Next.js (App Router) with TypeScript `strict`
- Database Target: PostgreSQL 16
- Validation: [Zod]
- Secrets live in the host environment or a secret store, never in git or the client bundle.

## Multi-Repo / API Gateways
- This size has one app and no gateway. Do not add a backend-for-frontend service.
- External API 1 Endpoint: `https://api.example.com/v1`
- Error body for Route Handlers: `{ "error": { "code": "ORDER_ALREADY_PAID", "message": "This order has already been paid.", "traceId": "..." } }`
- Unsafe creates (payments, webhooks) accept an `Idempotency-Key`.
