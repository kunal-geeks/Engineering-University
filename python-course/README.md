# Python Course — Foundations to Principal Engineer

A structured path from first script to the language depth, production engineering, and technical leadership expected at **principal engineer** level.

## Prerequisites

- Python 3.11+ recommended ([python.org/downloads](https://www.python.org/downloads/))
- Terminal, git, and a modern editor (Cursor, VS Code, or PyCharm)
- **Part I:** no prior Python required
- **Part II+:** complete Part I or equivalent experience (1–2 years writing Python)
- **Part IV:** experience shipping production systems; comfort with distributed systems concepts

Verify your install:

```bash
python3 --version
```

## How to Use This Course

1. Work through parts in order — later lessons assume earlier ones.
2. Type examples yourself; run and modify them.
3. Complete exercises before advancing; check [exercises/](exercises/).
4. For Part IV, treat lessons as frameworks for decisions you make on real systems — annotate with your own tradeoffs.

## Course Map

| Part | Level | Lessons | Outcome |
|------|-------|---------|---------|
| **I** | Junior → Mid | 1–12 | Write correct, idiomatic Python |
| **II** | Mid → Senior | 13–22 | Master the language, runtime, and concurrency model |
| **III** | Senior → Staff | 23–30 | Design, ship, and operate production Python at scale |
| **IV** | Staff → Principal | 31–36 | Architecture, reliability, org impact, technical strategy |

---

## Part I — Foundations (Lessons 1–12)

| # | Lesson | Topics |
|---|--------|--------|
| 1 | [Getting Started](lessons/01-getting-started.md) | REPL, scripts, indentation |
| 2 | [Variables and Types](lessons/02-variables-and-types.md) | Numbers, strings, booleans, conversion |
| 3 | [Control Flow](lessons/03-control-flow.md) | `if`, loops, comprehensions |
| 4 | [Functions](lessons/04-functions.md) | Scope, `*args`, lambdas |
| 5 | [Data Structures](lessons/05-data-structures.md) | Lists, tuples, dicts, sets |
| 6 | [File I/O](lessons/06-file-io.md) | Files, `pathlib`, CSV, JSON |
| 7 | [Modules and Packages](lessons/07-modules-and-packages.md) | Imports, packages, `pip` |
| 8 | [Object-Oriented Programming](lessons/08-object-oriented-programming.md) | Classes, inheritance, dataclasses |
| 9 | [Error Handling](lessons/09-error-handling.md) | Exceptions, tracebacks, debugging |
| 10 | [Virtual Environments](lessons/10-virtual-environments-and-pip.md) | `venv`, dependencies |
| 11 | [Standard Library Highlights](lessons/11-standard-library-highlights.md) | `json`, `datetime`, `argparse` |
| 12 | [Intermediate Projects](lessons/12-projects-and-next-steps.md) | Capstones, bridge to Part II |

---

## Part II — Advanced Language & Runtime (Lessons 13–22)

| # | Lesson | Topics |
|---|--------|--------|
| 13 | [Iterators & Generators](lessons/13-iterators-and-generators.md) | Protocol, `yield`, `yield from`, pipelines |
| 14 | [Decorators & Descriptors](lessons/14-decorators-and-descriptors.md) | `@wraps`, descriptor protocol, properties |
| 15 | [Metaclasses & Object Model](lessons/15-metaclasses-and-object-model.md) | MRO, `__init_subclass__`, when *not* to use metaclasses |
| 16 | [Advanced Type System](lessons/16-advanced-type-system.md) | `Protocol`, `Generic`, `TypeVar`, `overload`, `mypy` |
| 17 | [Context Managers](lessons/17-context-managers.md) | `contextlib`, async contexts, resource safety |
| 18 | [Memory, GIL & Profiling](lessons/18-memory-gil-and-profiling.md) | Object model, `__slots__`, `cProfile`, allocation |
| 19 | [Threading & Multiprocessing](lessons/19-threading-and-multiprocessing.md) | GIL implications, pools, shared state |
| 20 | [Asyncio Deep Dive](lessons/20-asyncio-deep-dive.md) | Tasks, streams, structured concurrency, pitfalls |
| 21 | [Testing Architecture](lessons/21-testing-architecture.md) | pytest fixtures, mocks, contracts, Hypothesis |
| 22 | [Packaging & Build Systems](lessons/22-packaging-and-build-systems.md) | `pyproject.toml`, wheels, uv/hatch, monorepos |

---

## Part III — Production Engineering at Scale (Lessons 23–30)

| # | Lesson | Topics |
|---|--------|--------|
| 23 | [API Design & Services](lessons/23-api-design-and-services.md) | FastAPI, Pydantic v2, versioning, pagination |
| 24 | [Data Layer Patterns](lessons/24-data-layer-patterns.md) | SQLAlchemy 2.0, transactions, migrations, repositories |
| 25 | [Observability](lessons/25-observability.md) | Structured logging, metrics, tracing, OpenTelemetry |
| 26 | [Security Engineering](lessons/26-security-engineering.md) | Auth, secrets, OWASP, dependency supply chain |
| 27 | [Caching & Background Jobs](lessons/27-caching-and-background-jobs.md) | Redis, TTL strategies, Celery/ARQ, idempotent workers |
| 28 | [Distributed Systems Patterns](lessons/28-distributed-systems-patterns.md) | Consistency, sagas, outbox, eventual consistency |
| 29 | [Software Architecture](lessons/29-software-architecture.md) | Hexagonal, DDD, CQRS, event-driven boundaries |
| 30 | [Performance at Scale](lessons/30-performance-at-scale.md) | Profiling in prod, batching, connection pools, N+1 |

---

## Part IV — Principal Engineer (Lessons 31–36)

| # | Lesson | Topics |
|---|--------|--------|
| 31 | [Reliability Engineering](lessons/31-reliability-engineering.md) | SLOs/SLIs, retries, circuit breakers, bulkheads |
| 32 | [System Design Case Studies](lessons/32-system-design-case-studies.md) | Rate limiters, search, payments, notification pipelines |
| 33 | [Architecture Governance](lessons/33-architecture-governance.md) | ADRs, RFCs, standards, deprecation strategy |
| 34 | [Code Review at Scale](lessons/34-code-review-at-scale.md) | Review culture, linters, CI gates, mentorship |
| 35 | [Principal Engineer Mindset](lessons/35-principal-engineer-mindset.md) | Tradeoffs, multi-team impact, tech strategy |
| 36 | [Capstone & Career Synthesis](lessons/36-capstone-and-career-synthesis.md) | Portfolio projects, interview loops, ongoing growth |

---

## Competency Rubric

Use this to self-assess after each part:

| Competency | Part I | Part II | Part III | Part IV |
|------------|--------|---------|----------|---------|
| Idiomatic Python | ✓ | ✓ | ✓ | ✓ |
| Language internals | | ✓ | ✓ | ✓ |
| Concurrency model | | ✓ | ✓ | ✓ |
| Production operations | | | ✓ | ✓ |
| Cross-service design | | | ✓ | ✓ |
| Org-level technical leadership | | | | ✓ |

## Quick Reference

```python
# Type-safe service boundary (Part II–III)
from typing import Protocol

class Notifier(Protocol):
    def send(self, user_id: str, message: str) -> None: ...

# Structured log (Part III)
import structlog
log = structlog.get_logger()
log.info("order_placed", order_id="ord_123", amount_cents=4999)

# Retry with backoff (Part IV)
import tenacity

@tenacity.retry(stop=tenacity.stop_after_attempt(3), wait=tenacity.wait_exponential())
def call_upstream(): ...
```

## License

Use and share freely for learning.
