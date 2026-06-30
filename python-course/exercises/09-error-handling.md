# Exercise Solutions — Lesson 9

## 1. parse_positive_int

```python
def parse_positive_int(text: str) -> int:
    try:
        value = int(text)
    except ValueError:
        raise ValueError(f"not a valid integer: {text!r}") from None
    if value <= 0:
        raise ValueError(f"must be positive, got {value}")
    return value
```

## 2. Safe file read

```python
from pathlib import Path

def read_file(path):
    try:
        return Path(path).read_text(encoding="utf-8")
    except FileNotFoundError:
        return None
```

## 3. Custom exception

```python
class InsufficientFundsError(Exception):
    pass


def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError(
            f"cannot withdraw {amount} from balance {balance}"
        )
    return balance - amount
```

## 4. Logging processor

```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)


def process_items(items):
    for item in items:
        logger.debug("Processing item: %s", item)
        # process item...
```
