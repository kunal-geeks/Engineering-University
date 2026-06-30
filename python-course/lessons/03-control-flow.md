# Lesson 3: Control Flow

## Conditional Statements

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

print(grade)
```

Ternary expression (compact `if`):

```python
status = "pass" if score >= 60 else "fail"
```

## Loops

### `for` loop

Iterate over a sequence:

```python
for letter in "abc":
    print(letter)

for i in range(5):       # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2):  # 2, 4, 6, 8
    print(i)
```

### `while` loop

Repeat while a condition is true:

```python
count = 3
while count > 0:
    print(count)
    count -= 1
```

### Loop control

```python
for n in range(10):
    if n == 3:
        continue    # skip to next iteration
    if n == 7:
        break       # exit loop
    print(n)
else:
    print("loop completed without break")
```

## Comprehensions

Build lists, dicts, and sets concisely:

```python
squares = [x ** 2 for x in range(6)]
# [0, 1, 4, 9, 16, 25]

evens = [x for x in range(10) if x % 2 == 0]
# [0, 2, 4, 6, 8]

word_lengths = {word: len(word) for word in ["hi", "hello"]}
# {'hi': 2, 'hello': 5}

unique_lengths = {len(w) for w in ["a", "bb", "ccc"]}
# {1, 2, 3}
```

## `match` / `case` (Python 3.10+)

Pattern matching for multiple branches:

```python
def describe(value):
    match value:
        case 0:
            return "zero"
        case 1 | 2:
            return "one or two"
        case int(n) if n < 0:
            return "negative integer"
        case str(s):
            return f"string of length {len(s)}"
        case _:
            return "something else"
```

## Practical Example: FizzBuzz

```python
for i in range(1, 21):
    if i % 15 == 0:
        print("FizzBuzz")
    elif i % 3 == 0:
        print("Fizz")
    elif i % 5 == 0:
        print("Buzz")
    else:
        print(i)
```

## Key Takeaways

- Use `if` / `elif` / `else` for branching.
- `for` loops iterate sequences; `while` loops repeat on a condition.
- List comprehensions are idiomatic for transforming sequences.
- `break`, `continue`, and `else` on loops give fine-grained control.

## Exercises

1. Write a program that prints whether a number is positive, negative, or zero.
2. Print the sum of all numbers from 1 to 100 using a `for` loop.
3. Build a list of squares for even numbers only from 1 to 20 using a comprehension.
4. Implement FizzBuzz for numbers 1–30 and store results in a list instead of printing.

## Solutions

See [exercises/03-control-flow.md](../exercises/03-control-flow.md).
