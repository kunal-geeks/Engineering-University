# Lesson 13: Iterators and Generators

Senior engineers treat iteration as a protocol, not a syntax feature. Understanding iterators unlocks memory-efficient pipelines and composable data processing.

## The Iterator Protocol

Any object with `__iter__()` returning `self` and `__next__()` raising `StopIteration` is an iterator:

```python
class Countdown:
    def __init__(self, start):
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value

list(Countdown(3))  # [3, 2, 1]
```

Iterable vs iterator: iterables have `__iter__()`; iterators are stateful and exhaust after one pass.

## Generators

Generators are iterators created with `yield`:

```python
def fibonacci(limit):
    a, b = 0, 1
    while a < limit:
        yield a
        a, b = b, a + b

list(fibonacci(100))  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

Generator expressions mirror list comprehensions but lazy:

```python
squares = (x * x for x in range(10_000_000))  # O(1) memory
next(squares)
```

## yield from — Delegation

```python
def chain(*iterables):
    for it in iterables:
        yield from it

list(chain([1, 2], [3, 4]))  # [1, 2, 3, 4]
```

`yield from` also propagates `send()`, `throw()`, and `close()` — critical for coroutine plumbing.

## itertools — Production Pipelines

```python
from itertools import islice, chain, groupby

def read_batches(items, size):
    it = iter(items)
    while batch := list(islice(it, size)):
        yield batch

# Group consecutive equal keys (requires sorted input)
data = sorted(["a1", "a2", "b1"], key=lambda x: x[0])
for key, group in groupby(data, key=lambda x: x[0]):
    print(key, list(group))
```

## send() and Coroutine Generators

```python
def accumulator():
    total = 0
    while True:
        value = yield total
        if value is None:
            break
        total += value

acc = accumulator()
next(acc)       # prime
acc.send(10)    # 10
acc.send(5)     # 15
```

This pattern predates `async def` and informs how async coroutines work internally.

## Memory and Performance

| Approach | Memory | When to use |
|----------|--------|-------------|
| List | O(n) | Small data, random access |
| Generator | O(1) | Streams, large files, pipelines |
| `itertools` | O(1) | Composable lazy transforms |

Reading a 10 GB log file:

```python
def error_lines(path):
    with open(path, encoding="utf-8") as f:
        for line in f:
            if "ERROR" in line:
                yield line.strip()

for line in error_lines("app.log"):
    process(line)
```

## Anti-Patterns

- Materializing huge generators with `list()` unnecessarily
- Reusing exhausted iterators
- Using generators when you need len(), indexing, or multiple passes

## Key Takeaways

- Iteration is a protocol; generators implement it cheaply.
- Prefer lazy pipelines for I/O-bound and large datasets.
- `yield from` delegates iteration and coroutine control flow.
- Know when laziness helps and when you need a concrete collection.

## Exercises

1. Write a generator ` sliding_window(seq, n)` yielding tuples of size `n`.
2. Implement `flatten(nested)` using `yield from` for arbitrary nesting depth.
3. Parse a large CSV lazily and compute running average without loading all rows.
4. Explain when you'd choose a list vs generator for a 50M-row ETL job.

## Solutions

See [exercises/13-iterators-and-generators.md](../exercises/13-iterators-and-generators.md).

## Next

[Lesson 14: Decorators & Descriptors →](14-decorators-and-descriptors.md)
