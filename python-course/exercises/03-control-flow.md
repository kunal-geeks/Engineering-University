# Exercise Solutions — Lesson 3

## 1. Positive, negative, or zero

```python
num = int(input("Enter a number: "))

if num > 0:
    print("positive")
elif num < 0:
    print("negative")
else:
    print("zero")
```

## 2. Sum 1 to 100

```python
total = 0
for i in range(1, 101):
    total += i
print(total)  # 5050
```

## 3. Even squares comprehension

```python
squares = [n ** 2 for n in range(1, 21) if n % 2 == 0]
# [4, 16, 36, 64, 100, 144, 196, 256, 324, 400]
```

## 4. FizzBuzz list

```python
def fizzbuzz(n):
    if n % 15 == 0:
        return "FizzBuzz"
    if n % 3 == 0:
        return "Fizz"
    if n % 5 == 0:
        return "Buzz"
    return str(n)

results = [fizzbuzz(i) for i in range(1, 31)]
```
