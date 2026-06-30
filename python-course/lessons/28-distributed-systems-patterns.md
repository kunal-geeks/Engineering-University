# Lesson 28: Distributed Systems Patterns

Python services rarely run alone. Principal engineers reason about consistency, failure, and messaging across process and network boundaries.

## CAP Theorem (Practical View)

During a partition, choose **Consistency** or **Availability**. Most web apps choose availability with **eventual consistency** for non-critical paths.

## Consistency Models

| Model | Example |
|-------|---------|
| Strong | Bank balance after commit |
| Eventual | Search index, analytics |
| Read-your-writes | Session stickiness or primary reads after write |

Document which operations require which guarantee.

## Message Queues vs Event Streams

| Tool | Pattern | Use case |
|------|---------|----------|
| Redis/RabbitMQ | Task queue | Job processing |
| Kafka/Pulsar | Event log | Audit trail, stream processing |
| SNS/SQS | Cloud queue | Managed async decoupling |

## Outbox Pattern

Guarantee DB write + message publish atomically:

```python
def place_order(order):
    with transaction():
        db.save(order)
        db.save(Outbox(event="order_placed", payload=order.id))
    # separate worker reads outbox, publishes to queue, marks sent
```

Avoids "saved to DB but message never sent" race.

## Saga Pattern — Distributed Transactions

Choreography (events) vs orchestration (coordinator):

```
OrderCreated → ReserveInventory → ChargePayment → ShipOrder
                    ↓ fail
              ReleaseInventory (compensating action)
```

Each step has a **compensating transaction** for rollback.

## Idempotency Everywhere

- HTTP: `Idempotency-Key` header
- Messages: dedupe by `message_id`
- Workers: check processed table before side effects

## Distributed Locks (Use Sparingly)

```python
# Redis Redlock — only when necessary
with redis_lock("job:daily_report"):
    generate_report()
```

Prefer idempotent design over locks when possible. Locks fail under network partitions.

## Circuit Breaker

```python
from circuitbreaker import circuit

@circuit(failure_threshold=5, recovery_timeout=30)
def call_payment_gateway():
    ...
```

Stop hammering failing dependencies; fail fast and degrade gracefully.

## Eventual Consistency UX

Tell users "processing" instead of lying about immediate consistency. Poll or push updates when index catches up.

## Key Takeaways

- Prefer outbox + idempotent consumers over dual writes.
- Sagas with compensations for multi-service workflows.
- Circuit breakers and bulkheads limit blast radius.
- Explicit consistency requirements per operation — not one-size-fits-all.

## Exercises

1. Design outbox table schema and worker flow for `order_placed` events.
2. Sketch saga steps and compensations for a checkout flow.
3. Add circuit breaker around an external API with fallback response.
4. Write consistency requirements doc for read vs write paths in your system.

## Solutions

See [exercises/28-distributed-systems-patterns.md](../exercises/28-distributed-systems-patterns.md)

## Next

[Lesson 29: Software Architecture →](29-software-architecture.md)
