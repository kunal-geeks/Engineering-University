# Exercise Solutions — Lesson 23

## ADR: URL versioning

Use lesson 23 ADR template. Recommend `/v1/` prefix for explicit routing, CDN cache keys, and parallel v1/v2 deployment during migration.

## Idempotency sketch

```python
async def create_payment(body, idempotency_key: str, redis, db):
    cached = await redis.get(f"idempotency:{idempotency_key}")
    if cached:
        return json.loads(cached)
    result = await db.create_payment(body)
    await redis.setex(f"idempotency:{idempotency_key}", 86400, json.dumps(result))
    return result
```
