# Exercise Solutions — Lesson 5

## 1. Sorted unique values

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6]
unique_sorted = sorted(set(nums))
# [1, 2, 3, 4, 5, 6, 9]
```

## 2. Country code dict

```python
countries = {
    "US": "United States",
    "GB": "United Kingdom",
    "IN": "India",
}
```

## 3. Merge word counts

```python
def merge_counts(a: dict, b: dict) -> dict:
    result = dict(a)
    for word, count in b.items():
        result[word] = result.get(word, 0) + count
    return result
```

## 4. Highest score name

```python
scores = [("Ada", 95), ("Grace", 92), ("Alan", 88)]

def top_scorer(entries):
    return max(entries, key=lambda item: item[1])[0]

top_scorer(scores)  # "Ada"
```
