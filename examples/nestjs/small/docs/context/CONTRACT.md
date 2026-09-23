# Environment, External Systems & API Contracts

## Environment Templates
```env
# DO NOT PASTE REAL SECRETS HERE. USE TEMPLATE KEYS ONLY.
DATABASE_URL=postgresql://user:pass@localhost:5432/db
API_SECRET_KEY=your_placeholder_secret_key
PORT=3000
LOG_LEVEL=info
```

## System Versions
- Runtime Version: Node.js 22.x
- Framework: NestJS with TypeScript `strict`
- Database Target: PostgreSQL 16
- Validation: class-validator and class-transformer
- Secrets are injected by the environment. Never commit a real `.env`.

## Multi-Repo / API Gateways
- One service, no gateway. Do not split modules onto the network.
- External API 1 Endpoint: `https://api.example.com/v1`
- Error body: `{ "error": { "code": "ORDER_ALREADY_PAID", "message": "This order has already been paid.", "traceId": "..." } }`
- List endpoints are paginated. Unsafe POSTs accept `Idempotency-Key`.
- OpenAPI is generated from the DTOs.
