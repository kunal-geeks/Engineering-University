# Lesson 2: Variables and Types

## Variables

A variable is a name that refers to a value.

```python
x = 10
message = "hello"
is_active = True
```

Python is **dynamically typed**: you do not declare types explicitly; they are inferred at runtime.

## Core Built-in Types

| Type | Example | Description |
|------|---------|-------------|
| `int` | `42`, `-7` | Whole numbers |
| `float` | `3.14`, `-0.5` | Decimal numbers |
| `str` | `"hello"`, `'world'` | Text |
| `bool` | `True`, `False` | Boolean values |
| `None` | `None` | Absence of a value |

Check a value's type:

```python
type(42)        # <class 'int'>
type(3.14)      # <class 'float'>
type("hi")      # <class 'str'>
```

## Numbers

```python
a = 10 + 3      # 13
b = 10 - 3      # 7
c = 10 * 3      # 30
d = 10 / 3      # 3.333... (float division)
e = 10 // 3     # 3 (floor division)
f = 10 % 3      # 1 (remainder)
g = 2 ** 8      # 256 (exponent)
```

## Strings

```python
first = "Ada"
last = 'Lovelace'
full = first + " " + last           # concatenation
greeting = f"Hello, {first}!"       # f-string (preferred)
multiline = """Line one
Line two"""

len(greeting)       # length
greeting.lower()    # "hello, ada!"
"  trim  ".strip()  # "trim"
"hello".replace("l", "L")  # "heLLo"
```

Indexing and slicing:

```python
word = "Python"
word[0]      # 'P'
word[-1]     # 'n'
word[0:3]    # 'Pyt'
word[2:]     # 'thon'
```

## Booleans and Comparisons

```python
5 > 3        # True
5 == 5       # True
5 != 3       # True
"a" in "cat" # True

# Logical operators
True and False   # False
True or False    # True
not True         # False
```

Truthy and falsy values: empty strings, `0`, empty collections, and `None` are falsy; most other values are truthy.

## Type Conversion

```python
int("42")       # 42
float("3.14")   # 3.14
str(100)        # "100"
bool(0)         # False
bool("text")    # True
```

Invalid conversions raise errors:

```python
int("hello")  # ValueError
```

## Assignment Patterns

```python
x = y = 0           # multiple assignment
a, b = 1, 2         # unpacking
a, b = b, a         # swap
count = count + 1   # or: count += 1
```

## Constants (Convention)

Python has no true constants. By convention, use `UPPER_SNAKE_CASE` for values that should not change:

```python
MAX_RETRIES = 3
PI = 3.14159
```

## Key Takeaways

- Python has rich built-in types: numbers, strings, booleans, and `None`.
- Use f-strings for readable string formatting.
- Compare values with `==`, `!=`, `<`, `>`, and logical operators.
- Convert types explicitly when needed.

## Exercises

1. Create variables for your name (str), age (int), and height in meters (float). Print them in one f-string.
2. Given `text = "  Learning Python  "`, strip whitespace and print it in uppercase.
3. Convert the string `"19.99"` to a float, add `0.01`, and print the result as a string with two decimal places (hint: f-string formatting `{value:.2f}`).

## Solutions

See [exercises/02-variables-and-types.md](../exercises/02-variables-and-types.md).
