# Exercise Solutions — Lesson 30

## Performance budgets example

| Route | p95 | Max queries |
|-------|-----|-------------|
| GET /v1/products | 200ms | 2 |
| GET /v1/products/{id} | 100ms | 1 |
| POST /v1/checkout | 800ms | 8 |

## Stale cache postmortem outline

**Impact:** users saw old prices for 2h  
**Root cause:** cache invalidation missing on bulk price job  
**Fix:** versioned cache keys + job publishes `price_version` bump event  
**Prevention:** integration test asserting cache bust after price update
