# Lesson 5: Data Structures

## Lists

Ordered, mutable sequences:

```python
fruits = ["apple", "banana", "cherry"]
fruits[0]           # "apple"
fruits.append("date")
fruits.insert(1, "apricot")
fruits.remove("banana")
last = fruits.pop()
len(fruits)

# Slicing (returns a new list)
fruits[1:3]
fruits[:2]
fruits[::-1]        # reversed copy
```

List methods: `extend`, `sort`, `reverse`, `index`, `count`, `clear`.

## Tuples

Ordered, **immutable** sequences:

```python
point = (10, 20)
x, y = point        # unpacking
single = (42,)      # note the comma for a one-item tuple
```

Use tuples for fixed collections of values (coordinates, RGB colors, database rows).

## Dictionaries

Key-value mappings:

```python
user = {
    "name": "Ada",
    "age": 36,
    "role": "engineer",
}

user["name"]            # "Ada"
user.get("email", "n/a")  # default if missing
user["email"] = "ada@example.com"
del user["role"]

for key in user:
    print(key, user[key])

for key, value in user.items():
    print(key, value)
```

Dictionary comprehensions:

```python
{char: ord(char) for char in "abc"}
```

## Sets

Unordered collections of **unique** values:

```python
tags = {"python", "tutorial", "python"}
# {'python', 'tutorial'}

tags.add("course")
tags.discard("tutorial")

a = {1, 2, 3}
b = {3, 4, 5}
a | b    # union: {1, 2, 3, 4, 5}
a & b    # intersection: {3}
a - b    # difference: {1, 2}
```

## Choosing a Structure

| Need | Use |
|------|-----|
| Ordered collection, may change | `list` |
| Fixed bundle of fields | `tuple` |
| Lookup by key | `dict` |
| Unique items, membership tests | `set` |

## Nested Structures

```python
students = [
    {"name": "Ada", "grades": [95, 88, 92]},
    {"name": "Grace", "grades": [90, 94, 91]},
]

students[0]["grades"].append(97)
```

## Practical Example: Word Frequency

```python
def word_count(text):
    counts = {}
    for word in text.lower().split():
        word = word.strip(".,!?")
        counts[word] = counts.get(word, 0) + 1
    return counts

word_count("To be or not to be")
# {'to': 2, 'be': 2, 'or': 1, 'not': 1}
```

## Key Takeaways

- Lists are mutable; tuples are immutable.
- Dicts map keys to values; sets store unique items.
- Comprehensions and unpacking make working with collections concise.
- Pick the structure that matches your access patterns.

## Exercises

1. Given `nums = [3, 1, 4, 1, 5, 9, 2, 6]`, return a sorted list of unique values.
2. Build a dict mapping country codes (`"US"`, `"GB"`, `"IN"`) to country names.
3. Write `merge_counts(a, b)` that merges two word-count dicts by summing values for shared keys.
4. Given a list of tuples `(name, score)`, return the name with the highest score.

## Solutions

See [exercises/05-data-structures.md](../exercises/05-data-structures.md).
