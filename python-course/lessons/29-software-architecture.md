# Lesson 29: Software Architecture

Architecture is the set of decisions that are hard to change. Principal engineers align structure with team boundaries, change frequency, and operational reality.

## Hexagonal Architecture (Ports & Adapters)

```
         ┌─────────────────────────┐
  HTTP ──┤                         ├── Postgres
  CLI  ──┤      Domain Core       ├── Redis
  Jobs ──┤   (pure business logic) ├── Email API
         └─────────────────────────┘
              ↑ ports (interfaces)
              ↓ adapters (implementations)
```

Domain depends on nothing external; adapters implement ports.

```python
# port
class PaymentGateway(Protocol):
    def charge(self, amount_cents: int, customer_id: str) -> str: ...

# adapter
class StripeGateway:
    def charge(self, amount_cents, customer_id):
        return stripe.Charge.create(...)

# domain
class CheckoutService:
    def __init__(self, payments: PaymentGateway):
        self.payments = payments
```

## Domain-Driven Design (Tactical)

| Building block | Purpose |
|----------------|---------|
| Entity | Object with identity (`Order`, `User`) |
| Value Object | Immutable, no identity (`Money`, `Email`) |
| Aggregate | Consistency boundary (`Order` + `LineItems`) |
| Repository | Persist aggregates |
| Domain Event | `OrderPlaced`, `PaymentFailed` |

One transaction per aggregate — cross-aggregate consistency via events.

## CQRS (When Justified)

Separate **Command** (writes) and **Query** (reads) models when read patterns diverge radically ( dashboards, search).

Don't adopt CQRS for CRUD — complexity cost is high.

## Event-Driven Architecture

```python
@dataclass(frozen=True)
class OrderPlaced:
    order_id: str
    user_id: str
    total_cents: int

def handle_order_placed(event: OrderPlaced):
    send_confirmation_email(event.user_id)
    update_analytics(event)
```

Events enable decoupling but require schema evolution discipline (versioned payloads).

## Modular Monolith vs Microservices

| Factor | Modular monolith | Microservices |
|--------|------------------|---------------|
| Team size | Small–medium | Multiple autonomous teams |
| Ops maturity | Lower | Requires strong platform |
| Deploy coupling | Single unit | Independent |
| Complexity | Lower cross-service | Network, distributed tracing |

**Start monolith modular**; extract services when boundaries and ops justify it.

## Python Package Boundaries

```
src/
├── billing/       # bounded context
│   ├── domain/
│   ├── application/
│   └── infrastructure/
└── shared/        # minimal — resist the junk drawer
```

Enforce import rules with tools like `import-linter`.

## Key Takeaways

- Domain core has no framework imports; adapters at the edge.
- DDD aggregates define transaction and consistency boundaries.
- Choose microservices for org/scale reasons, not hype.
- Explicit module boundaries beat accidental coupling.

## Exercises

1. Refactor a god-module into domain + adapter layers with a Protocol port.
2. Identify aggregates in an e-commerce domain; document invariants.
3. Draw a context map for three bounded contexts and their integration style.
4. Write an ADR: monolith vs microservices for your product stage.

## Solutions

See [exercises/29-software-architecture.md](../exercises/29-software-architecture.md)

## Next

[Lesson 30: Performance at Scale →](30-performance-at-scale.md)
