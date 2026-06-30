# Exercise Solutions — Lesson 17

## 1. Temporary log level

```python
import logging
from contextlib import contextmanager

@contextmanager
def log_level(level):
    logger = logging.getLogger()
    old = logger.level
    logger.setLevel(level)
    try:
        yield
    finally:
        logger.setLevel(old)
```

## 2. transaction context manager

```python
@contextmanager
def transaction(session):
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
```

## 3. ExitStack for N files

```python
from contextlib import ExitStack

def read_all(paths):
    with ExitStack() as stack:
        files = [stack.enter_context(open(p, encoding="utf-8")) for p in paths]
        return [f.read() for f in files]
```
