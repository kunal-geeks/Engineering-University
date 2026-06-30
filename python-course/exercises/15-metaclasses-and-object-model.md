# Exercise Solutions — Lesson 15

## 1. Diamond MRO

```python
class A:
    def greet(self): return "A"

class B(A):
    def greet(self): return "B"

class C(A):
    def greet(self): return "C"

class D(B, C):
    pass

# D.__mro__ → (D, B, C, A, object)
# D().greet() → "B" (left-first in MRO)
```

## 2. Plugin registration

```python
class PluginBase:
    registry: dict[str, type] = {}

    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        name = getattr(cls, "name", cls.__name__)
        PluginBase.registry[name] = cls

class EmailPlugin(PluginBase):
    name = "email"
```

## 3. __slots__ memory

Use `tracemalloc` or `sys.getsizeof` on batches — expect ~40–50% reduction per instance for simple objects at 1M scale. Exact numbers vary by Python version and fields.

## 4. Django model metaclass

Django's `ModelBase` metaclass registers models with the app registry, collects `Field` objects from class namespace, and builds database table metadata at import time.
