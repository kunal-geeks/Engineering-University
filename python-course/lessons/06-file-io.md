# Lesson 6: File I/O

## Reading and Writing Text Files

Always use a `with` block so files close automatically:

```python
# Write
with open("notes.txt", "w", encoding="utf-8") as f:
    f.write("Line one\n")
    f.write("Line two\n")

# Read entire file
with open("notes.txt", "r", encoding="utf-8") as f:
    content = f.read()

# Read line by line (memory-efficient for large files)
with open("notes.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())
```

## File Modes

| Mode | Meaning |
|------|---------|
| `"r"` | Read (default) |
| `"w"` | Write (creates or overwrites) |
| `"a"` | Append |
| `"r+"` | Read and write |

Always specify `encoding="utf-8"` for text files.

## Working with Paths

Use `pathlib` for modern path handling:

```python
from pathlib import Path

data_dir = Path("data")
data_dir.mkdir(exist_ok=True)

file_path = data_dir / "report.txt"
file_path.write_text("Hello\n", encoding="utf-8")
text = file_path.read_text(encoding="utf-8")

for path in data_dir.glob("*.txt"):
    print(path.name, path.stat().st_size)
```

## CSV Files

```python
import csv

rows = [
    ["name", "score"],
    ["Ada", "95"],
    ["Grace", "92"],
]

with open("scores.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerows(rows)

with open("scores.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["name"], row["score"])
```

## JSON Files

```python
import json

data = {"name": "Ada", "skills": ["python", "math"]}

with open("profile.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2)

with open("profile.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)
```

## Error Handling with Files

```python
from pathlib import Path

path = Path("missing.txt")
if path.exists():
    print(path.read_text(encoding="utf-8"))
else:
    print("File not found")
```

## Practical Example: Log Analyzer

```python
from pathlib import Path

def count_errors(log_path):
    count = 0
    with open(log_path, encoding="utf-8") as f:
        for line in f:
            if "ERROR" in line:
                count += 1
    return count
```

## Key Takeaways

- Use `with open(...)` for safe file handling.
- Prefer `pathlib.Path` over string path concatenation.
- Use the `csv` and `json` modules for structured data.
- Always set `encoding="utf-8"` for text files.

## Exercises

1. Write a script that creates `journal.txt` and appends today's date and one sentence.
2. Read a text file and print the number of lines and words.
3. Write a list of dicts to `users.json` and read it back.
4. Use `pathlib` to list all `.py` files in the current directory.

## Solutions

See [exercises/06-file-io.md](../exercises/06-file-io.md).
