# Exercise Solutions — Lesson 4

## 1–4. Functions with type hints and docstrings

```python
def is_palindrome(s: str) -> bool:
    """Return True if s reads the same forwards and backwards (case-insensitive)."""
    cleaned = s.lower()
    return cleaned == cleaned[::-1]


def average(*numbers: float) -> float | None:
    """Return the arithmetic mean of numbers, or None if empty."""
    if not numbers:
        return None
    return sum(numbers) / len(numbers)


def repeat(text: str, times: int = 1) -> str:
    """Return text repeated times (default 1)."""
    return text * times
```

Usage:

```python
is_palindrome("Racecar")   # True
average(2, 4, 6)           # 4.0
average()                  # None
repeat("ha", 3)            # "hahaha"
```
