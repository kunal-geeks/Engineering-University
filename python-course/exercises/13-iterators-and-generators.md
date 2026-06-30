# Exercise Solutions — Lesson 13

## 1. sliding_window

```python
from collections import deque
from typing import Iterable, TypeVar

T = TypeVar("T")

def sliding_window(seq: Iterable[T], n: int):
    if n < 1:
        raise ValueError("n must be >= 1")
    window: deque[T] = deque(maxlen=n)
    for item in seq:
        window.append(item)
        if len(window) == n:
            yield tuple(window)
```

## 2. flatten

```python
def flatten(nested):
    for item in nested:
        if isinstance(item, (list, tuple)):
            yield from flatten(item)
        else:
            yield item
```

## 3. Running average (lazy CSV)

```python
import csv

def running_averages(path, column="value"):
    total, count = 0.0, 0
    with open(path, newline="", encoding="utf-8") as f:
        for row in csv.DictReader(f):
            total += float(row[column])
            count += 1
            yield total / count
```

## 4. List vs generator (50M rows)

Use a **generator pipeline**: O(1) memory, single pass. A list requires O(n) memory (~ hundreds of MB+). Choose list only if you need random access, multiple passes, or `len()` without counting.
