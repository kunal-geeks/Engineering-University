# Exercise Solutions — Lesson 6

## 1. Journal append

```python
from datetime import date
from pathlib import Path

path = Path("journal.txt")
with path.open("a", encoding="utf-8") as f:
    f.write(f"{date.today()}: Learning Python file I/O.\n")
```

## 2. Line and word counts

```python
from pathlib import Path

def analyze(path):
    text = Path(path).read_text(encoding="utf-8")
    lines = text.splitlines()
    words = text.split()
    return len(lines), len(words)
```

## 3. JSON users

```python
import json
from pathlib import Path

users = [
    {"name": "Ada", "role": "engineer"},
    {"name": "Grace", "role": "admiral"},
]

Path("users.json").write_text(json.dumps(users, indent=2), encoding="utf-8")
loaded = json.loads(Path("users.json").read_text(encoding="utf-8"))
```

## 4. List .py files with pathlib

```python
from pathlib import Path

for path in Path(".").glob("*.py"):
    print(path.name)
```
