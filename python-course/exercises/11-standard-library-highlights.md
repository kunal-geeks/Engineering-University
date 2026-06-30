# Exercise Solutions — Lesson 11

## 1. Top three words with Counter

```python
from collections import Counter

paragraph = """
Python is a programming language. Python is readable and versatile.
Programming in Python is productive.
"""

words = paragraph.lower().split()
for word, count in Counter(words).most_common(3):
    print(word, count)
```

## 2. Day of week from date string

```python
from datetime import datetime

d = datetime.strptime("2024-12-25", "%Y-%m-%d")
print(d.strftime("%A"))  # Wednesday
```

## 3. Count .py files recursively

```python
from pathlib import Path

def count_py_files(root):
    return sum(1 for p in Path(root).rglob("*.py") if p.is_file())

count_py_files(".")
```

## 4. CLI line counter

```python
import argparse
from pathlib import Path

parser = argparse.ArgumentParser()
parser.add_argument("filename")
args = parser.parse_args()

lines = Path(args.filename).read_text(encoding="utf-8").splitlines()
print(len(lines))
```

Run: `python linecount.py README.md`
