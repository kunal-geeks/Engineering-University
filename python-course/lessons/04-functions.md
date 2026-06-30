# Lesson 4: Functions

## Defining and Calling Functions

```python
def greet(name):
    return f"Hello, {name}!"

message = greet("Ada")
print(message)
```

Functions without an explicit `return` give `None`.

## Parameters

### Positional and keyword arguments

```python
def connect(host, port, timeout=30):
    return f"{host}:{port} (timeout={timeout})"

connect("localhost", 8080)
connect("localhost", port=8080, timeout=10)
connect(port=443, host="example.com")
```

### Arbitrary arguments

```python
def total(*args):
    return sum(args)

total(1, 2, 3)  # 6

def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Ada", role="engineer")
```

### Unpacking at call site

```python
nums = [1, 2, 3]
total(*nums)

config = {"host": "localhost", "port": 8000}
connect(**config)
```

## Type Hints (Optional but Recommended)

```python
def add(a: int, b: int) -> int:
    return a + b
```

Hints document intent; Python does not enforce them at runtime without external tools like `mypy`.

## Scope

```python
x = "global"

def demo():
    x = "local"
    print(x)

demo()   # "local"
print(x) # "global"
```

Use `global` sparingly; prefer passing values and returning results.

## Lambda Functions

Small anonymous functions:

```python
double = lambda n: n * 2
sorted(names, key=lambda n: len(n))
```

Use named `def` functions for anything non-trivial.

## Docstrings

Document functions with triple-quoted strings immediately after the definition:

```python
def area(radius):
    """Return the area of a circle given its radius."""
    return 3.14159 * radius ** 2
```

Access with `help(area)` or `area.__doc__`.

## First-Class Functions

Functions are objects — pass them around like any value:

```python
def apply(fn, value):
    return fn(value)

apply(lambda x: x * 10, 5)  # 50
```

## Practical Example

```python
def celsius_to_fahrenheit(celsius: float) -> float:
    return celsius * 9 / 5 + 32

def convert_all(temps_c):
    return [celsius_to_fahrenheit(t) for t in temps_c]

convert_all([0, 20, 37])
# [32.0, 68.0, 98.6]
```

## Key Takeaways

- Define reusable logic with `def`; return values with `return`.
- Default parameters, `*args`, and `**kwargs` handle flexible APIs.
- Type hints and docstrings improve readability and tooling.
- Prefer clear named functions over complex lambdas.

## Exercises

1. Write `is_palindrome(s)` that returns `True` if a string reads the same forwards and backwards (ignore case).
2. Write `average(*numbers)` that returns the mean or `None` if no numbers are passed.
3. Write `repeat(text, times=1)` with a default of one repetition.
4. Add type hints and a docstring to each function.

## Solutions

See [exercises/04-functions.md](../exercises/04-functions.md).
