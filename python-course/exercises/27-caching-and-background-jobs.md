# Exercise Solutions — Lesson 27

## Idempotent worker

```python
def process_event(event_id: str, payload: dict, db):
    if db.event_processed(event_id):
        return
    with db.transaction():
        do_work(payload)
        db.mark_processed(event_id)
```

## Catalog cache strategy

- **Product metadata:** Redis TTL 15 min + delete-on-write on admin update
- **Prices:** TTL 5 min OR versioned key `catalog:v{price_version}` bumped on bulk price job
- **Stale acceptable:** show "price confirmed at checkout" disclaimer
