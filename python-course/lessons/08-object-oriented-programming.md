# Lesson 8: Object-Oriented Programming

## Classes and Objects

A class defines structure and behavior; an instance is a concrete object.

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says woof!"

buddy = Dog("Buddy", 3)
print(buddy.bark())
```

## Instance vs Class Attributes

```python
class Counter:
    total_created = 0  # class attribute

    def __init__(self):
        Counter.total_created += 1
        self.count = 0   # instance attribute

    def increment(self):
        self.count += 1
```

## Methods

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("amount must be positive")
        self.balance += amount

    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("insufficient funds")
        self.balance -= amount

    def __str__(self):
        return f"{self.owner}: ${self.balance:.2f}"
```

## Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError

class Cat(Animal):
    def speak(self):
        return f"{self.name} says meow"

class Dog(Animal):
    def speak(self):
        return f"{self.name} says woof"
```

Call parent methods with `super()`:

```python
class Employee:
    def __init__(self, name):
        self.name = name

class Manager(Employee):
    def __init__(self, name, department):
        super().__init__(name)
        self.department = department
```

## Dataclasses (Python 3.7+)

Reduce boilerplate for data-holding classes:

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: float
    y: float

    def distance_from_origin(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

p = Point(3, 4)
print(p)  # Point(x=3, y=4)
```

Options: `@dataclass(frozen=True)` for immutability.

## Encapsulation (Convention)

Python uses naming conventions rather than strict access control:

```python
class Account:
    def __init__(self):
        self.balance = 0      # public
        self._cache = {}      # "internal use"
        self.__secret = 42    # name mangling (rare)
```

## Special Methods (Dunder)

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
```

## Key Takeaways

- Classes bundle data (`self` attributes) and behavior (methods).
- Use inheritance to share behavior; override methods in subclasses.
- Dataclasses are ideal for simple data containers.
- Special methods customize how objects behave with operators and built-ins.

## Exercises

1. Create a `Rectangle` class with `width`, `height`, `area()`, and `perimeter()`.
2. Subclass `Rectangle` as `Square` that sets width and height equal in `__init__`.
3. Convert a `Book` class (title, author, pages) to a dataclass with type hints.
4. Add `__repr__` to `Rectangle` so printing it shows useful info.

## Solutions

See [exercises/08-object-oriented-programming.md](../exercises/08-object-oriented-programming.md).
