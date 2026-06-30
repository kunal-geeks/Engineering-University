# Lesson 30: Performance at Scale

Performance work at principal level is systematic: measure production, prioritize by user impact, and fix the constraint — not every microsecond.

## Production Profiling

- **APM** — Datadog, New Relic, Pyroscope (continuous profiling)
- **Traces** — find slow spans in distributed calls
- **DB** — `pg_stat_statements`, slow query logs

Never optimize from local benchmarks alone.

## Database Performance

```python
# Batch inserts
session.bulk_insert_mappings(User, rows)  # vs 1000 individual adds

# Pagination — keyset not offset
stmt = select(User).where(User.id > last_id).order_by(User.id).limit(50)
```

Index columns in `WHERE`, `JOIN`, `ORDER BY`. Explain analyze slow queries.

## HTTP and API Efficiency

- Compression (gzip/brotli)
- Partial responses / field selection
- HTTP caching headers for immutable resources
- Connection reuse (`httpx.AsyncClient` as singleton)

## Batching External Calls

```python
async def fetch_users(ids: list[str]):
    # one query
    return await repo.get_many(ids)
    # not: [await repo.get(i) for i in ids]
```

Amortize network round trips.

## Caching Revisited

Cache at the right layer:

- CDN for static assets
- Redis for hot read models
- Application memoization for pure expensive computes (`lru_cache` with bounds)

## Async Worker Scaling

Scale horizontally:

```
Load balancer → N Uvicorn workers → shared Redis/Postgres
```

CPU-bound Python → more processes, not bigger machines alone.

## Memory at Scale

- Stream large responses
- Paginate internal processing
- Avoid loading full tables into memory
- Use generators and chunked DB reads

## Load Testing

```bash
pip install locust
# locustfile.py — simulate user behavior
locust -f locustfile.py --host=https://staging.example.com
```

Establish baseline before releases; compare p50/p95/p99 latency.

## Performance Budget

Define budgets per endpoint:

| Endpoint | p95 latency | Max DB queries |
|----------|-------------|----------------|
| GET /products | 200ms | 2 |
| POST /checkout | 800ms | 8 |

CI can fail load tests that breach budgets on staging.

## Key Takeaways

- Find the constraint (DB, network, CPU) with production data.
- Batch, paginate, index — boring fixes win at scale.
- Load test continuously; track p95/p99, not just averages.
- Performance is a feature with explicit budgets.

## Exercises

1. Identify and fix an N+1 in a staging environment; measure query count before/after.
2. Write a Locust test for your highest-traffic endpoint.
3. Define performance budgets for three critical API routes.
4. Document a production incident where caching caused stale data — root cause and fix.

## Solutions

See [exercises/30-performance-at-scale.md](../exercises/30-performance-at-scale.md)

## Next

[Part IV: Reliability Engineering →](31-reliability-engineering.md)
