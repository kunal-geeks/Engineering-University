# Lesson 31: Reliability Engineering

Principal engineers define how reliable systems should be — and build mechanisms (SLOs, graceful degradation, chaos) to get there.

## SLIs, SLOs, and Error Budgets

| Term | Definition | Example |
|------|------------|---------|
| **SLI** | Measurable signal | Request success rate |
| **SLO** | Target for SLI | 99.9% success / 30 days |
| **Error budget** | Allowed unreliability | 0.1% ≈ 43 min downtime/month |

When budget is exhausted → freeze features, focus on reliability.

## Retry with Exponential Backoff

```python
import tenacity

@tenacity.retry(
    retry=tenacity.retry_if_exception_type(TransientError),
    wait=tenacity.wait_exponential(multiplier=1, min=1, max=60),
    stop=tenacity.stop_after_attempt(5),
)
def call_upstream():
    ...
```

Add **jitter** to prevent thundering herds:

```python
wait=tenacity.wait_random_exponential(multiplier=1, max=60)
```

Retry only **idempotent** operations or with idempotency keys.

## Circuit Breaker States

```
Closed (normal) → failures exceed threshold → Open (fail fast)
Open → timeout → Half-open (probe) → success → Closed
```

Combine with fallbacks:

```python
def get_recommendations(user_id):
    try:
        return rec_service.fetch(user_id)
    except CircuitOpenError:
        return get_cached_or_default(user_id)
```

## Bulkheads

Isolate resources so one failing dependency doesn't exhaust the pool:

```python
payment_pool = ThreadPoolExecutor(max_workers=5)
analytics_pool = ThreadPoolExecutor(max_workers=20)
```

Separate connection pools per downstream service.

## Graceful Degradation

| Feature | Degraded mode |
|---------|---------------|
| Recommendations | Show popular items |
| Real-time sync | Queue for later |
| Search facets | Basic text search only |

Document degraded behavior in runbooks.

## Health Checks

```python
@app.get("/health/live")
async def liveness():
    return {"status": "ok"}

@app.get("/health/ready")
async def readiness():
    await db.execute(text("SELECT 1"))
    await redis.ping()
    return {"status": "ready"}
```

Liveness = process up. Readiness = can serve traffic.

## Chaos Engineering (Intro)

Controlled failure injection in staging:

- Kill random pods
- Inject latency to dependencies
- Fill disk / exhaust connections

Validate alerts and runbooks before production incidents.

## Postmortems — Blameless

Template:

1. Summary and impact
2. Timeline
3. Root cause (5 whys)
4. What went well / poorly
5. Action items with owners

## Key Takeaways

- SLOs align engineering and product on reliability tradeoffs.
- Retries, circuit breakers, and bulkheads are standard patterns — implement deliberately.
- Health checks and runbooks enable safe operations.
- Error budgets connect reliability to release velocity.

## Exercises

1. Define SLI/SLO/error budget for a payment API.
2. Implement retry with jitter for a transient HTTP client.
3. Add liveness/readiness endpoints with DB dependency check.
4. Write a blameless postmortem template and fill it for a hypothetical outage.

## Solutions

See [exercises/31-reliability-engineering.md](../exercises/31-reliability-engineering.md)

## Next

[Lesson 32: System Design Case Studies →](32-system-design-case-studies.md)
