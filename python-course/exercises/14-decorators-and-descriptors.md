# Exercise Solutions — Lesson 14

## 1. log_args decorator

```python
import logging
from functools import wraps

def log_args(fn):
    logger = logging.getLogger(fn.__module__)

    @wraps(fn)
    def wrapper(*args, **kwargs):
        logger.debug("calling %s args=%s kwargs=%s", fn.__name__, args, kwargs)
        return fn(*args, **kwargs)
    return wrapper
```

## 2. TypedField descriptor

```python
class TypedField:
    def __init__(self, expected_type):
        self.expected_type = expected_type
        self.name = None

    def __set_name__(self, owner, name):
        self.name = f"_{name}"

    def __get__(self, obj, objtype=None):
        return getattr(obj, self.name)

    def __set__(self, obj, value):
        if not isinstance(value, self.expected_type):
            raise TypeError(f"expected {self.expected_type}, got {type(value)}")
        setattr(obj, self.name, value)
```

## 3. deprecated decorator

```python
import warnings
from functools import wraps

def deprecated(reason=""):
    def decorator(fn):
        msg = f"{fn.__name__} is deprecated. {reason}".strip()

        @wraps(fn)
        def wrapper(*args, **kwargs):
            warnings.warn(msg, DeprecationWarning, stacklevel=2)
            return fn(*args, **kwargs)
        return wrapper
    return decorator
```

## 4. Why @wraps matters

Without `@wraps`, decorated functions expose the wrapper's `__name__`, `__doc__`, and signature. Stack traces show `wrapper` instead of the endpoint name; OpenAPI introspection, pytest node IDs, and APM transaction names break across 200+ routes.
