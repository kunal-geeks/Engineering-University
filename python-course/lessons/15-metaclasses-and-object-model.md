# Lesson 15: Metaclasses and the Object Model

Metaclasses are "classes that create classes." Most code never needs them — but principal engineers understand the object model to debug frameworks (Django, SQLAlchemy, Pydantic) and know when simpler hooks suffice.

## How Classes Are Created

```python
class Dog:
    species = "Canis"
    def bark(self):
        return "woof"
```

Roughly equivalent to:

```python
Dog = type("Dog", (), {"species": "Canis", "bark": lambda self: "woof"})
```

`type(name, bases, namespace)` is the default metaclass.

## Method Resolution Order (MRO)

```python
class A:
    def greet(self): return "A"

class B(A):
    def greet(self): return "B"

class C(A):
    def greet(self): return "C"

class D(B, C):
    pass

D.__mro__  # (D, B, C, A, object)
D().greet()  # "B"
```

Use `super()` correctly in cooperative inheritance:

```python
class B(A):
    def greet(self):
        return super().greet() + " -> B"
```

## __init_subclass__ — Prefer This Over Metaclasses

```python
class PluginBase:
    registry = {}

    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        if cls.__name__ != "PluginBase":
            PluginBase.registry[cls.__name__] = cls

class EmailPlugin(PluginBase):
    pass

PluginBase.registry  # {'EmailPlugin': <class '...'>}
```

`__init_subclass__` handles registration, validation, and config without metaclass complexity.

## __slots__ — Memory and Attribute Control

```python
class Point:
    __slots__ = ("x", "y")
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

- Removes per-instance `__dict__` (~40% memory savings for millions of objects)
- Prevents arbitrary attribute assignment
- Complicates multiple inheritance — use deliberately

## A Minimal Metaclass

```python
class EnforceTypes(type):
    def __new__(mcs, name, bases, namespace):
        for key, value in namespace.items():
            if key.startswith("_"):
                continue
            # illustrative only — real systems use annotations + validators
        return super().__new__(mcs, name, bases, namespace)
```

## When Frameworks Use Metaclasses

| Framework | Purpose |
|-----------|---------|
| Django models | Register models, build SQL mappings |
| SQLAlchemy declarative | Map classes to tables |
| ABC module | `ABCMeta` enforces abstract methods |
| Enum | Creates enum members |

## dataclass Internals (Conceptual)

`@dataclass` generates `__init__`, `__repr__`, `__eq__` at class creation time via a decorator that mutates the class — not a metaclass, but same phase of execution.

## Anti-Patterns

- Metaclasses for validation Pydantic already handles
- Magic behavior invisible at call sites
- Overriding `__getattribute__` without extreme need (infinite recursion risk)

## Debugging the Object Model

```python
import inspect

inspect.getmro(cls)
cls.__dict__
vars(instance)
descriptor = getattr(type(instance), "attr", None)
```

## Key Takeaways

- Classes are objects; metaclasses create them — default is `type`.
- MRO and `super()` matter in multiple inheritance.
- Prefer `__init_subclass__`, class decorators, and descriptors before metaclasses.
- Reach for metaclasses when building frameworks, not application code.

## Exercises

1. Draw the MRO for a diamond inheritance pattern and predict method output.
2. Implement plugin registration with `__init_subclass__` and a `name` class variable.
3. Compare memory usage of 1M instances with and without `__slots__`.
4. Read Django model source and identify one metaclass responsibility.

## Solutions

See [exercises/15-metaclasses-and-object-model.md](../exercises/15-metaclasses-and-object-model.md).

## Next

[Lesson 16: Advanced Type System →](16-advanced-type-system.md)
