# Lesson 27: Caching and Background Jobs

Caching and async work define scalability ceilings. Principal engineers choose strategies that stay correct under failure, not just fast in the happy path.

## Caching Layers

```
Client → CDN → API cache → Application cache (Redis) → Database
```

Cache closer to the user for static assets; application cache for computed/read-heavy data.

## Redis Patterns

```python
import redis
import json

r = redis.Redis.from_url(REDIS_URL)

def get_user_cached(user_id: str) -> dict:
    key = f"user:{user_id}"
    cached = r.get(key)
    if cached:
        return json.loads(cached)
    user = load_user_from_db(user_id)
    r.setex(key, 300, json.dumps(user))  # TTL 5 min
    return user
```

## Cache Invalidation Strategies

| Strategy | When |
|----------|------|
| TTL only | Stale data acceptable |
| Write-through | Update cache on write |
| Delete on write | Invalidate key on mutation |
| Versioned keys | `user:123:v2` on schema change |

"There are only two hard things..." — plan invalidation in the design doc.

## Stampede Protection

```python
def get_with_lock(key, loader, ttl=300):
    if val := r.get(key):
        return json.loads(val)
    with r.lock(f"lock:{key}", timeout=10):
        if val := r.get(key):  # double-check
            return json.loads(val)
        val = loader()
        r.setex(key, ttl, json.dumps(val))
        return val
```

## Background Jobs — When to Use

Move off the request path:

- Email/notifications
- Report generation
- Image processing
- Webhook delivery with retries

## Celery / ARQ Overview

```python
# tasks.py (Celery)
from celery import Celery

app = Celery("worker", broker=REDIS_URL)

@app.task(bind=True, max_retries=3)
def send_email(self, to, subject, body):
    try:
        mailer.send(to, subject, body)
    except MailError as exc:
        raise self.retry(exc=exc, countdown=60)
```

ARQ is asyncio-native and lighter for async stacks.

## Idempotent Workers

```python
def process_payment(payment_id: str):
    if already_processed(payment_id):
        return  # safe retry
    with transaction():
        charge(payment_id)
        mark_processed(payment_id)
```

Assume **at-least-once delivery** — duplicates will happen.

## Dead Letter Queues

Failed jobs after max retries → DLQ for manual inspection and replay tooling.

## Scheduled Jobs

Use Celery Beat, APScheduler, or cloud schedulers (EventBridge, Cloud Scheduler).

Document timezone handling explicitly (UTC everywhere in code).

## Key Takeaways

- Every cache needs an invalidation story and TTL rationale.
- Protect against stampedes and stale reads on critical data.
- Background jobs must be idempotent and observable.
- DLQs and retry policies are production requirements.

## Exercises

1. Add Redis caching to a read endpoint with delete-on-write invalidation.
2. Implement idempotent task processing with a processed-keys table.
3. Configure retries with exponential backoff for a flaky external API call.
4. Design cache strategy for a product catalog with hourly price updates.

## Solutions

See [exercises/27-caching-and-background-jobs.md](../exercises/27-caching-and-background-jobs.md)

## Next

[Lesson 28: Distributed Systems Patterns →](28-distributed-systems-patterns.md)
