# Lesson 1: Getting Started

## What Is Python?

Python is a general-purpose programming language known for readable syntax and a large ecosystem of libraries. It is widely used for web development, data science, automation, scripting, and more.

## Running Python

### The REPL (Read-Eval-Print Loop)

Open a terminal and start an interactive session:

```bash
python3
```

You will see a prompt like `>>>`. Type expressions and press Enter:

```python
>>> 2 + 2
4
>>> "hello".upper()
'HELLO'
```

Exit with `exit()` or `Ctrl+D` (macOS/Linux) / `Ctrl+Z` then Enter (Windows).

### Running a Script

Create a file named `hello.py`:

```python
print("Hello, world!")
```

Run it:

```bash
python3 hello.py
```

## Comments

Comments explain code to humans; Python ignores them.

```python
# This is a single-line comment

"""
This is a multi-line string, often used as a docstring
at the top of a module or function.
"""
```

## Indentation Matters

Python uses indentation (spaces, not tabs) to define blocks. Use **4 spaces** per level:

```python
if True:
    print("indented block")
```

Mixing tabs and spaces causes errors. Configure your editor to insert spaces.

## Basic Output

```python
print("Hello")
print("Name:", "Ada", "Age:", 36)
print("Score", 95, sep="=", end="!\n")
```

## Your First Program

Create `greet.py`:

```python
name = input("What is your name? ")
print(f"Nice to meet you, {name}!")
```

Run it and type your name when prompted.

## Key Takeaways

- Use `python3` in the terminal for the REPL or to run `.py` files.
- Indentation defines code blocks.
- `print()` displays output; `input()` reads user input.

## Exercises

1. Write a script that prints three lines: your name, favorite language, and why you are learning Python.
2. Use the REPL to evaluate `10 * 5`, `"py" * 3`, and `len("python")`.
3. Modify `greet.py` to ask for the user's city and print a welcome message that includes both name and city.

## Solutions

See [exercises/01-getting-started.md](../exercises/01-getting-started.md).
