# Lesson 12: Intermediate Projects (Bridge to Part II)

You have Python fundamentals. Before Part II, solidify them with a project and adopt habits that senior engineers use daily.

## Capstone Project Ideas

### Todo CLI (recommended)

- JSON persistence, `argparse` subcommands, dataclasses
- Tests with pytest; type hints throughout

### Expense Tracker

- CSV/JSON I/O, `datetime` grouping, summary reports

### HTTP API Client

- `httpx`, retry logic, config via env vars

## Professional Habits to Adopt Now

```bash
python -m venv .venv && source .venv/bin/activate
pip install pytest ruff mypy
ruff check . && ruff format .
mypy src/
pytest
```

Project layout:

```
myapp/
├── pyproject.toml
├── src/myapp/
│   ├── __init__.py
│   ├── cli.py
│   └── models.py
└── tests/
    └── test_models.py
```

## What Changes in Part II

Part I taught you to **write** Python. Part II teaches you to **reason about** Python:

- How iteration and generators actually work
- Descriptors, metaclasses, and the object model
- The GIL, asyncio, and when each concurrency model wins
- Type systems that scale across large codebases

## Readiness Checklist

Before starting [Lesson 13](13-iterators-and-generators.md):

- [ ] Comfortable with functions, classes, exceptions, and modules
- [ ] Can write pytest tests without looking up syntax
- [ ] Understand list/dict comprehensions and `with` blocks
- [ ] Completed at least one capstone project end-to-end

## Exercises

1. Build the Todo CLI with add/list/complete/delete and at least five tests.
2. Add `mypy --strict` to your project and fix all type errors.
3. Write a one-page README explaining setup, usage, and design decisions.

## Next

Continue to [Part II: Iterators & Generators →](13-iterators-and-generators.md)
