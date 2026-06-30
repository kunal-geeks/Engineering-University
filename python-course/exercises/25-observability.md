# Exercise Solutions — Lesson 25

## SLI/SLO example — POST /checkout

- **SLI:** proportion of successful responses (non-5xx) with latency < 800ms
- **SLO:** 99.5% over 30 days
- **Alert:** burn rate > 2% of monthly budget in 1 hour

## Alert rule (pseudo-PromQL)

```
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket{endpoint="/checkout"}[5m])) > 0.8
```

Page on sustained breach; ticket on single spike.
