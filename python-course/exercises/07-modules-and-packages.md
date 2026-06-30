# Exercise Solutions — Lesson 7

## 1–2. math_tools module

`math_tools.py`:

```python
"""Basic math utilities."""

def mean(numbers):
    if not numbers:
        return None
    return sum(numbers) / len(numbers)


def main():
    print("mean([1, 2, 3]) =", mean([1, 2, 3]))


if __name__ == "__main__":
    main()
```

`main.py`:

```python
from math_tools import mean

print(mean([10, 20, 30]))
```

## 3. Using rich

```bash
pip install rich
```

```python
from rich import print

print("[bold green]Hello[/bold green]")
```

## 4. calc package

```
calc/
├── __init__.py
├── basic.py
└── advanced.py
```

`calc/basic.py`:

```python
def add(a, b):
    return a + b
```

`calc/advanced.py`:

```python
def power(base, exp):
    return base ** exp
```

Usage:

```python
from calc.basic import add
from calc.advanced import power
```
