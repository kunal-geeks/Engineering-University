# Lesson 16: Advanced Type System

Static typing at scale is a principal-engineer tool: it documents contracts, enables safe refactors, and catches integration bugs before production.

## Gradual Typing Philosophy

Python types are **optional** and checked by external tools (`mypy`, `pyright`). Runtime behavior is unchanged.

```bash
pip install mypy
mypy --strict src/
```

## Protocol — Structural Subtyping

```python
from typing import Protocol

class Readable(Protocol):
    def read(self, n: int = -1) -> str: ...

def load(source: Readable) -> str:
    return source.read()

load(open("file.txt"))  # OK — file objects match structurally
```

No inheritance required. Ideal for dependency injection and test doubles.

## Generic Types

```python
from typing import TypeVar, Generic

T = TypeVar("T")

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: list[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

stack: Stack[int] = Stack()
stack.push(1)
```

Python 3.12+ syntax: `class Stack[T]:`

## Bounded and Constrained TypeVars

```python
from typing import TypeVar

import numbers
Num = TypeVar("Num", bound=numbers.Number)

def double(x: Num) -> Num:
    return x * 2  # type: ignore[return-value] — illustrative
```

## TypedDict — Dicts with Known Keys

```python
from typing import TypedDict, NotRequired

class UserRow(TypedDict):
    id: int
    name: str
    email: NotRequired[str]

def create_user(row: UserRow) -> None:
    ...
```

## Literal, Union, and Narrowing

```python
from typing import Literal, Union

Mode = Literal["dev", "staging", "prod"]

def connect(mode: Mode) -> None:
    if mode == "prod":
        enable_tls()

def parse_id(value: Union[int, str]) -> int:
    if isinstance(value, int):
        return value
    return int(value)
```

Python 3.10+: use `int | str` instead of `Union`.

## overload — Different Signatures

```python
from typing import overload

@overload
def process(x: int) -> int: ...
@overload
def process(x: str) -> str: ...

def process(x: int | str) -> int | str:
    if isinstance(x, int):
        return x * 2
    return x.upper()
```

## ParamSpec and Callable — Higher-Order Functions

```python
from typing import Callable, ParamSpec, TypeVar

P = ParamSpec("P")
R = TypeVar("R")

def logged(fn: Callable[P, R]) -> Callable[P, R]:
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
        print(f"calling {fn.__name__}")
        return fn(*args, **kwargs)
    return wrapper
```

## NewType and Branded IDs

```python
from typing import NewType

UserId = NewType("UserId", str)
OrderId = NewType("OrderId", str)

def get_user(user_id: UserId) -> dict: ...

get_user(UserId("u_123"))   # OK
get_user(OrderId("o_456"))  # mypy error — prevents ID mixups
```

## Pydantic vs TypedDict vs dataclass

| Tool | Runtime validation | Static typing | Best for |
|------|-------------------|---------------|----------|
| dataclass | No | Yes | Internal models |
| TypedDict | No | Yes (dict-shaped JSON) | Parsed JSON blobs |
| Pydantic | Yes | Yes | API boundaries, config |

## CI Integration

```yaml
# .github/workflows/ci.yml (excerpt)
- run: mypy --strict src/
- run: pyright src/
```

## Key Takeaways

- `Protocol` enables duck typing with static checks.
- Generics and `NewType` prevent category errors at scale.
- Run `mypy --strict` in CI; treat type errors as build failures.
- Use Pydantic at system boundaries; TypedDict/dataclass internally.

## Exercises

1. Define a `Cache` Protocol and implement in-memory and no-op backends.
2. Create `Stack[T]` with full generic typing and mypy-clean tests.
3. Use `NewType` for `Email`, `UserId`, and prevent cross-assignment.
4. Add `--strict` mypy to a project and document suppressions with `# type: ignore[code]` only where justified.

## Solutions

See [exercises/16-advanced-type-system.md](../exercises/16-advanced-type-system.md).

## Next

[Lesson 17: Context Managers →](17-context-managers.md)
