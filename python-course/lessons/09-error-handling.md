# Lesson 9: Error Handling

## Exceptions

When something goes wrong, Python raises an exception. Uncaught exceptions stop the program with a traceback.

```python
int("not a number")  # ValueError
10 / 0               # ZeroDivisionError
items[99]            # IndexError (if list is shorter)
```

## try / except

Handle expected errors gracefully:

```python
def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None
```

Catch specific exceptions — avoid bare `except:`.

```python
try:
    value = int(user_input)
except ValueError:
    print("Please enter a valid integer")
```

## Multiple except Blocks

```python
try:
    process(data)
except ValueError as e:
    print(f"Bad value: {e}")
except FileNotFoundError:
    print("File missing")
except Exception as e:
    print(f"Unexpected error: {e}")
```

Use `Exception` as a last resort, not the first catch-all.

## else and finally

```python
try:
    result = risky_operation()
except RuntimeError:
    result = default_value
else:
    print("Succeeded:", result)   # runs if no exception
finally:
    cleanup()                       # always runs
```

## Raising Exceptions

```python
def withdraw(balance, amount):
    if amount <= 0:
        raise ValueError("amount must be positive")
    if amount > balance:
        raise ValueError("insufficient funds")
    return balance - amount
```

Create custom exceptions for domain errors:

```python
class ValidationError(Exception):
    pass

def set_age(age):
    if age < 0:
        raise ValidationError("age cannot be negative")
```

## Assertions

For programmer errors and internal checks (can be disabled with `-O`):

```python
assert len(items) > 0, "items must not be empty"
```

Do not use assertions for user input validation.

## Reading Tracebacks

A traceback shows the call stack from bottom (root cause) to top (where error surfaced):

```
Traceback (most recent call last):
  File "app.py", line 10, in main
    result = parse(value)
  File "app.py", line 3, in parse
    return int(text)
ValueError: invalid literal for int() with base 10: 'abc'
```

Start at the bottom for the error type and message; read upward for context.

## Debugging Tips

```python
# Quick inspection
print(repr(variable))

# Built-in breakpoint (Python 3.7+)
breakpoint()

# Logging instead of print in production code
import logging
logging.basicConfig(level=logging.DEBUG)
logging.debug("value=%s", value)
```

## Practical Example

```python
def load_config(path):
    import json
    from pathlib import Path

    config_path = Path(path)
    if not config_path.exists():
        raise FileNotFoundError(f"Config not found: {path}")

    try:
        return json.loads(config_path.read_text(encoding="utf-8"))
    except json.JSONDecodeError as e:
        raise ValueError(f"Invalid JSON in {path}: {e}") from e
```

## Key Takeaways

- Catch specific exceptions you can handle; let others propagate.
- Use `raise` to signal invalid states clearly.
- `finally` ensures cleanup runs regardless of errors.
- Read tracebacks from the bottom up; use logging and `breakpoint()` when debugging.

## Exercises

1. Write `parse_positive_int(text)` that returns an int or raises `ValueError` with a clear message.
2. Wrap file reading in try/except for `FileNotFoundError` and return `None` on failure.
3. Create a custom `InsufficientFundsError` and use it in a simple bank `withdraw` function.
4. Add logging to a function that processes a list, logging each item at DEBUG level.

## Solutions

See [exercises/09-error-handling.md](../exercises/09-error-handling.md).
