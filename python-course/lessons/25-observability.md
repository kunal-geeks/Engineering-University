# Lesson 25: Observability

You cannot operate what you cannot see. Principal engineers treat logs, metrics, and traces as first-class product requirements.

## Three Pillars

| Pillar | Answers | Tools |
|--------|---------|-------|
| **Logs** | What happened? | structlog, ELK, Loki |
| **Metrics** | How much/how fast? | Prometheus, Datadog |
| **Traces** | Where did time go? | OpenTelemetry, Jaeger |

## Structured Logging

```python
import structlog

structlog.configure(
    processors=[
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.JSONRenderer(),
    ]
)
log = structlog.get_logger()

log.info("order_created", order_id="ord_123", user_id="u_456", amount_cents=4999)
```

Never parse free-text logs in production — use key-value fields.

## Log Levels — Discipline

| Level | Use |
|-------|-----|
| DEBUG | Development diagnostics |
| INFO | Normal business events |
| WARNING | Recoverable anomalies |
| ERROR | Failed operations needing attention |
| CRITICAL | Service degradation |

Log **once** at the boundary that handles the error — avoid duplicate stack traces.

## Correlation IDs

```python
import uuid
from contextvars import ContextVar

request_id: ContextVar[str] = ContextVar("request_id", default="")

@app.middleware("http")
async def add_request_id(request, call_next):
    rid = request.headers.get("X-Request-ID", str(uuid.uuid4()))
    request_id.set(rid)
    response = await call_next(request)
    response.headers["X-Request-ID"] = rid
    return response
```

Bind `request_id` to every log line in the request.

## Metrics with Prometheus

```python
from prometheus_client import Counter, Histogram

REQUESTS = Counter("http_requests_total", "Total HTTP requests", ["method", "endpoint", "status"])
LATENCY = Histogram("http_request_duration_seconds", "Request latency", ["endpoint"])

@LATENCY.labels("/users").time()
def handle_users():
    REQUESTS.labels("GET", "/users", "200").inc()
```

RED method: **Rate**, **Errors**, **Duration** per endpoint.

## OpenTelemetry Tracing

```python
from opentelemetry import trace
from opentelemetry.instrumentation.fastapi import FastAPIInstrumentor

tracer = trace.get_tracer(__name__)

async def process_order(order_id):
    with tracer.start_as_current_span("process_order") as span:
        span.set_attribute("order.id", order_id)
        await charge_payment(order_id)
        await send_confirmation(order_id)

FastAPIInstrumentor.instrument_app(app)
```

Traces show cross-service latency — essential for microservices.

## Dashboards and Alerts

Alert on **symptoms** (error rate, latency SLO breach), not every log line.

Example SLO: 99.9% of requests < 500ms over 30 days.

## Key Takeaways

- Structured logs with correlation IDs across services.
- Metrics for RED/USE; traces for distributed latency.
- Instrument at boundaries; avoid logging secrets/PII.
- Define SLOs and alert on user-visible degradation.

## Exercises

1. Replace print statements with structlog JSON logs including `request_id`.
2. Add Prometheus metrics to an API: request count and latency histogram.
3. Instrument one cross-service call with OpenTelemetry spans.
4. Define SLI/SLO for a critical endpoint and draft one alert rule.

## Solutions

See [exercises/25-observability.md](../exercises/25-observability.md)

## Next

[Lesson 26: Security Engineering →](26-security-engineering.md)
