# Exercise Solutions — Lesson 16

## 1. Cache Protocol

```python
from typing import Protocol, TypeVar

T = TypeVar("T")

class Cache(Protocol):
    def get(self, key: str) -> T | None: ...
    def set(self, key: str, value: T, ttl: int | None = None) -> None: ...

class MemoryCache:
    def __init__(self):
        self._data: dict[str, object] = {}

    def get(self, key: str):
        return self._data.get(key)

    def set(self, key: str, value, ttl=None):
        self._data[key] = value

class NoOpCache:
    def get(self, key: str):
        return None

    def set(self, key: str, value, ttl=None):
        pass
```

## 2–3. Stack and NewType

See lesson examples — verify with `mypy --strict`.

## 4. mypy suppressions

Only use `# type: ignore[specific-code]` with a comment explaining why (e.g. third-party stubs missing).
