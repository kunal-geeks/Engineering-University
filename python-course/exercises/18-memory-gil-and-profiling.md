# Exercise Solutions — Lesson 18

## 1. Fibonacci profiling

```python
from functools import lru_cache

def fib_naive(n):
    if n < 2:
        return n
    return fib_naive(n - 1) + fib_naive(n - 2)

@lru_cache(maxsize=None)
def fib_cached(n):
    if n < 2:
        return n
    return fib_cached(n - 1) + fib_cached(n - 2)
```

Run `python -m cProfile -s cumtime` — naive explodes exponentially; cached is O(n).

## 2. Thread vs process benchmark

Document expected results: threads match or beat sequential for I/O-bound; processes win for CPU-bound Python; threads don't speed CPU-bound due to GIL.

## 3. Memory comparison

Use `tracemalloc` snapshots after allocating 100k instances of slotted vs non-slotted classes.

## 4. Performance investigation doc template

Include: hypothesis, baseline profile, change, after profile, metric delta, decision.
