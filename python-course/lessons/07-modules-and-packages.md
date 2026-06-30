# Lesson 7: Modules and Packages

## What Is a Module?

A module is any `.py` file. Importing it runs its top-level code once and gives you access to its names.

Create `greetings.py`:

```python
"""Greeting utilities."""

def hello(name):
    return f"Hello, {name}!"

def goodbye(name):
    return f"Goodbye, {name}!"
```

Use it from another file:

```python
import greetings

print(greetings.hello("Ada"))

from greetings import goodbye
print(goodbye("Grace"))
```

## Import Styles

```python
import math
math.sqrt(16)

from math import sqrt, pi
sqrt(16)

import datetime as dt
dt.date.today()

from collections import defaultdict as dd
```

Avoid `from module import *` — it pollutes the namespace and hides origins.

## The `if __name__ == "__main__"` Pattern

```python
def main():
    print("Running as a script")

if __name__ == "__main__":
    main()
```

When a file is imported, `__name__` is the module name. When run directly, it is `"__main__"`. This lets you write reusable modules that also work as scripts.

## Packages

A package is a directory containing an `__init__.py` file (can be empty):

```
myapp/
├── __init__.py
├── utils.py
└── models/
    ├── __init__.py
    └── user.py
```

Import from packages:

```python
from myapp.utils import helper
from myapp.models.user import User
```

## Installing Third-Party Packages

```bash
pip install requests
```

Use in code:

```python
import requests

response = requests.get("https://api.github.com")
print(response.status_code)
```

## Project Layout (Small App)

```
todo-cli/
├── pyproject.toml      # or requirements.txt
├── README.md
└── todo/
    ├── __init__.py
    ├── cli.py
    └── storage.py
```

Run as a module:

```bash
python -m todo.cli
```

## Standard Library Highlights

Python ships with many modules — no install needed:

```python
import os
import sys
import json
import random
import datetime
from pathlib import Path
from collections import Counter
```

Browse the full list: [docs.python.org/3/library](https://docs.python.org/3/library/)

## Key Takeaways

- One file = one module; folders with `__init__.py` = packages.
- Use explicit imports and the `__main__` guard for script/module dual use.
- Install external libraries with `pip`.
- Prefer the standard library before reaching for third-party packages.

## Exercises

1. Create a module `math_tools.py` with `mean(numbers)` and import it from `main.py`.
2. Add a `if __name__ == "__main__"` block that runs a quick demo.
3. Install `rich` with pip and use it to print a colored "Hello" message.
4. Organize two modules into a package `calc/` with `basic.py` and `advanced.py`.

## Solutions

See [exercises/07-modules-and-packages.md](../exercises/07-modules-and-packages.md).
