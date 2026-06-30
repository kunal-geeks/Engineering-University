# Lesson 11: Standard Library Highlights

Python's standard library is large. These modules cover common daily tasks.

## json — JSON Serialization

```python
import json

data = {"name": "Ada", "active": True, "scores": [95, 88]}
text = json.dumps(data, indent=2)
restored = json.loads(text)
```

## datetime — Dates and Times

```python
from datetime import date, datetime, timedelta

today = date.today()
now = datetime.now()
future = today + timedelta(days=7)

formatted = now.strftime("%Y-%m-%d %H:%M:%S")
parsed = datetime.strptime("2024-06-15", "%Y-%m-%d")
```

Use timezone-aware datetimes for production apps (`datetime.timezone`, `zoneinfo`).

## pathlib — Paths

```python
from pathlib import Path

root = Path.home() / "Documents" / "projects"
for py_file in root.rglob("*.py"):
    if py_file.is_file():
        print(py_file.relative_to(root))
```

## collections — Specialized Containers

```python
from collections import Counter, defaultdict, deque

Counter("abracadabra")           # letter counts
# Counter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})

groups = defaultdict(list)
groups["fruit"].append("apple")

queue = deque([1, 2, 3])
queue.appendleft(0)
```

## itertools — Iterator Tools

```python
from itertools import chain, islice, product

list(chain([1, 2], [3, 4]))     # [1, 2, 3, 4]
list(islice(range(100), 5))     # [0, 1, 2, 3, 4]
list(product("AB", "12"))       # [('A','1'), ('A','2'), ...]
```

## functools — Function Utilities

```python
from functools import lru_cache, partial

@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
square(5)  # 25
```

## random — Randomness

```python
import random

random.randint(1, 6)
random.choice(["rock", "paper", "scissors"])
items = [1, 2, 3, 4, 5]
random.shuffle(items)
```

For cryptography, use `secrets` instead of `random`.

## re — Regular Expressions

```python
import re

pattern = r"\b\d{3}-\d{4}\b"
text = "Call 555-1234 or 555-5678"
re.findall(pattern, text)

match = re.search(r"(\w+)@(\w+\.\w+)", "ada@example.com")
if match:
    user, domain = match.groups()
```

## argparse — CLI Arguments

```python
import argparse

parser = argparse.ArgumentParser(description="Greet someone")
parser.add_argument("name", help="Name to greet")
parser.add_argument("--loud", action="store_true", help="Uppercase output")
args = parser.parse_args()

msg = f"Hello, {args.name}!"
print(msg.upper() if args.loud else msg)
```

Run: `python greet.py Ada --loud`

## logging — Structured Logs

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
)
logger = logging.getLogger(__name__)

logger.info("Application started")
logger.warning("Low disk space")
```

## Key Takeaways

- Reach for the standard library before installing packages.
- `json`, `datetime`, `pathlib`, and `collections` appear in most projects.
- Use `argparse` for CLI tools and `logging` instead of print in real apps.
- Read module docs at [docs.python.org/3/library](https://docs.python.org/3/library/)

## Exercises

1. Use `Counter` to find the three most common words in a paragraph.
2. Parse `"2024-12-25"` into a `date` and print the day of the week (hint: `strftime("%A")`).
3. Walk a directory tree with `pathlib` and count total `.py` files.
4. Build a CLI that accepts a filename and prints line count using `argparse`.

## Solutions

See [exercises/11-standard-library-highlights.md](../exercises/11-standard-library-highlights.md).
