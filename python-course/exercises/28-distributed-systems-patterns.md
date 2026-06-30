# Exercise Solutions — Lesson 28

## Outbox schema

```sql
CREATE TABLE outbox (
    id BIGSERIAL PRIMARY KEY,
    event_type VARCHAR NOT NULL,
    payload JSONB NOT NULL,
    created_at TIMESTAMPTZ DEFAULT now(),
    published_at TIMESTAMPTZ NULL
);
```

Worker: `SELECT ... WHERE published_at IS NULL FOR UPDATE SKIP LOCKED`, publish, set `published_at`.

## Checkout saga

| Step | Action | Compensation |
|------|--------|--------------|
| 1 | Reserve inventory | Release reservation |
| 2 | Charge payment | Refund |
| 3 | Create shipment | Cancel shipment |

Each step idempotent with business IDs.
