# Lesson 32: System Design Case Studies

Principal engineers walk into design reviews with frameworks, tradeoff vocabulary, and patterns from real systems. These case studies exercise that muscle.

## Framework for Any Design

1. **Requirements** — functional, scale, latency, consistency
2. **Estimates** — QPS, storage, bandwidth (order of magnitude)
3. **API** — core endpoints and data model
4. **High-level design** — components and data flow
5. **Deep dives** — bottlenecks, failure modes
6. **Tradeoffs** — what you chose and what you deferred

---

## Case Study 1: Rate Limiter

**Requirement:** 100 requests/minute per user across API cluster.

**Approach:** Token bucket in Redis

```
KEY: ratelimit:{user_id}
INCR with EXPIRE window OR sliding window log
```

| Algorithm | Pros | Cons |
|-----------|------|------|
| Fixed window | Simple | Burst at boundary |
| Sliding window | Smoother | More Redis ops |
| Token bucket | Allows bursts | Slightly complex |

Return `429` with `Retry-After` header. Fail open vs closed — document choice for outages.

---

## Case Study 2: Notification Pipeline

**Flow:**

```
Event → Queue → Worker → Email/SMS/Push providers
         ↓
    Dead letter queue
```

- Idempotent sends (`notification_id` dedupe)
- Template rendering in worker, not API path
- Provider-specific adapters behind `Notifier` Protocol
- Retry with backoff; DLQ after N failures

**Scale:** shard queues by user_id hash; horizontal workers.

---

## Case Study 3: Search Index

**Problem:** Full-text search on products; DB `LIKE` doesn't scale.

**Architecture:**

```
Write path: API → DB → outbox → indexer → Elasticsearch
Read path:  API → ES (eventually consistent)
```

- Dual-write avoided — outbox ensures index catches up
- Reindex job for mapping changes
- Search degradation: fallback to DB primary key lookup

---

## Case Study 4: Payment Processing

**Requirements:** strong consistency, audit trail, PCI scope minimization.

```
Checkout API → Payment service → Stripe adapter
              ↓
         Ledger DB (append-only)
              ↓
         Webhook handler (idempotent)
```

- Never store raw card data — tokenize with provider
- Webhooks verified with signature
- Reconciliation batch job daily

---

## Case Study 5: Multi-Tenant SaaS

**Isolation options:**

| Model | Isolation | Cost |
|-------|-----------|------|
| Shared schema + tenant_id | Row-level | Lowest |
| Schema per tenant | Medium | Medium |
| DB per tenant | Highest | Highest |

Python middleware sets tenant context:

```python
tenant_id: ContextVar[str]

@app.middleware("http")
async def tenant_middleware(request, call_next):
    tenant_id.set(resolve_tenant(request))
    ...
```

Enforce tenant_id in **every** query — test for cross-tenant leaks.

---

## Estimation Quick Reference

```
1 day ≈ 86,400 seconds
1M DAU × 10 req/day ≈ 115 QPS average (2–3× peak)
1 KB record × 100M records ≈ 100 GB
```

Always state assumptions.

## Key Takeaways

- Reuse patterns: queues, outbox, idempotency, caching, sharding.
- Every design has explicit tradeoffs — name what you didn't build.
- Security and consistency requirements drive architecture more than QPS.
- Practice whiteboard designs with timers (45 min).

## Exercises

1. Design a URL shortener — API, storage, redirect flow, analytics.
2. Design a real-time leaderboard for a game — consistency vs speed.
3. Choose tenant isolation model for a B2B SaaS; write ADR.
4. Mock 45-min system design: notification service end-to-end.

## Solutions

See [exercises/32-system-design-case-studies.md](../exercises/32-system-design-case-studies.md)

## Next

[Lesson 33: Architecture Governance →](33-architecture-governance.md)
