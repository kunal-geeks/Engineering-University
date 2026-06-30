# Lesson 17: Context Managers

Context managers guarantee setup/teardown — files closed, locks released, transactions committed or rolled back. Production Python relies on them everywhere.

## The Protocol

```python
class Managed:
    def __enter__(self):
        print("enter")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("exit")
        return False  # don't suppress exceptions

with Managed():
    print("body")
```

If `__exit__` returns `True`, exceptions are suppressed (rare — use deliberately).

## contextlib.contextmanager

```python
from contextlib import contextmanager

@contextmanager
def temporary_env(key, value):
    import os
    old = os.environ.get(key)
    os.environ[key] = value
    try:
        yield
    finally:
        if old is None:
            del os.environ[key]
        else:
            os.environ[key] = old
```

The `yield` splits setup (before) from teardown (after `finally`).

## Nested Contexts

```python
from contextlib import ExitStack

with ExitStack() as stack:
    f1 = stack.enter_context(open("a.txt"))
    f2 = stack.enter_context(open("b.txt"))
    lock = stack.enter_context(threading.Lock())
    # all cleaned up in reverse order
```

`ExitStack` is essential when the number of resources is dynamic.

## Async Context Managers

```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def db_session():
    session = await create_session()
    try:
        yield session
        await session.commit()
    except Exception:
        await session.rollback()
        raise
    finally:
        await session.close()

async with db_session() as session:
    await session.execute(...)
```

FastAPI's lifespan hooks use this pattern.

## Production Patterns

### Timed block

```python
@contextmanager
def timer(label: str):
    start = time.perf_counter()
    yield
    elapsed = time.perf_counter() - start
    logger.info("timing", label=label, seconds=elapsed)
```

### Database transaction

```python
@contextmanager
def transaction(conn):
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
```

### Suppress expected errors

```python
from contextlib import suppress

with suppress(FileNotFoundError):
    os.remove(temp_path)
```

## suppress vs try/except

Use `suppress` only for truly expected, benign failures. Never hide programming errors.

## Key Takeaways

- Context managers enforce RAII-style resource safety in Python.
- `contextlib.contextmanager` reduces boilerplate for simple cases.
- `ExitStack` handles dynamic resource counts.
- Async services need `async with` and `@asynccontextmanager`.

## Exercises

1. Write a `@contextmanager` that changes logging level temporarily.
2. Implement `transaction(session)` with commit/rollback semantics.
3. Use `ExitStack` to open N files from a list of paths.
4. Convert a try/finally cleanup block in your codebase to a context manager.

## Solutions

See [exercises/17-context-managers.md](../exercises/17-context-managers.md).

## Next

[Lesson 18: Memory, GIL & Profiling →](18-memory-gil-and-profiling.md)
