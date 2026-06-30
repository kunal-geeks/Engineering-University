# Lesson 14: Decorators and Descriptors

Decorators and descriptors are how Python implements `@property`, ORMs, validation, and cross-cutting concerns. Principal engineers recognize when these tools clarify design vs obscure it.

## Functions as Decorators

```python
from functools import wraps
import time

def timed(fn):
    @wraps(fn)  # preserves __name__, __doc__, signature
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = fn(*args, **kwargs)
        print(f"{fn.__name__} took {time.perf_counter() - start:.4f}s")
        return result
    return wrapper

@timed
def compute():
    return sum(range(1_000_000))
```

**Always use `@wraps`** — without it, stack traces and introspection break.

## Decorators with Arguments

```python
from functools import wraps

def retry(times=3):
    def decorator(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            last_exc = None
            for _ in range(times):
                try:
                    return fn(*args, **kwargs)
                except Exception as e:
                    last_exc = e
            raise last_exc
        return wrapper
    return decorator

@retry(times=5)
def flaky(): ...
```

## Class-Based Decorators

```python
class CountCalls:
    def __init__(self, fn):
        self.fn = fn
        self.count = 0

    def __call__(self, *args, **kwargs):
        self.count += 1
        return self.fn(*args, **kwargs)
```

## Stacking Decorators

```python
@route("/users")
@require_auth
@rate_limit(100)
def list_users(): ...
```

Applied bottom-up: `route(require_auth(rate_limit(list_users)))`.

## The Descriptor Protocol

Objects defining `__get__`, `__set__`, or `__delete__` control attribute access:

```python
class Validator:
    def __init__(self, min_val, max_val):
        self.min_val = min_val
        self.max_val = max_val
        self.name = None

    def __set_name__(self, owner, name):
        self.name = f"_{name}"

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return getattr(obj, self.name, None)

    def __set__(self, obj, value):
        if not self.min_val <= value <= self.max_val:
            raise ValueError(f"{value} out of range")
        setattr(obj, self.name, value)

class Sensor:
    reading = Validator(0, 100)
```

`property` is a data descriptor. Functions used as methods are non-data descriptors (implement `__get__` only).

## Data vs Non-Data Descriptors

Lookup order on instances:

1. Data descriptor on class (`__set__` defined)
2. Instance `__dict__`
3. Non-data descriptor on class
4. Class `__dict__`
5. `__getattr__` if defined

This is why `@classmethod` and `@staticmethod` work.

## Practical Patterns

### Cached property

```python
from functools import cached_property

class Config:
    @cached_property
    def expensive(self):
        return load_from_remote()
```

### Authorization decorator

```python
def require_role(role):
    def decorator(fn):
        @wraps(fn)
        def wrapper(user, *args, **kwargs):
            if role not in user.roles:
                raise PermissionError
            return fn(user, *args, **kwargs)
        return wrapper
    return decorator
```

## When Not to Use Decorators

- Hiding complex business logic — prefer explicit function calls
- Stacking more than 3–4 decorators on one function — refactor
- Decorators that mutate global state unpredictably

## Key Takeaways

- Decorators are syntactic sugar for higher-order functions; use `@wraps`.
- Descriptors power attributes; understand lookup order for debugging ORMs.
- Prefer simple, named abstractions over clever meta-programming.
- Principal-level code optimizes for readability of call sites.

## Exercises

1. Write `@log_args` that logs function name and kwargs at DEBUG level.
2. Implement a `TypedField` descriptor that coerces to `int` or `str`.
3. Build `@deprecated(reason)` that emits `DeprecationWarning`.
4. Explain why `@wraps` matters in a codebase with 200+ decorated endpoints.

## Solutions

See [exercises/14-decorators-and-descriptors.md](../exercises/14-decorators-and-descriptors.md).

## Next

[Lesson 15: Metaclasses & Object Model →](15-metaclasses-and-object-model.md)
