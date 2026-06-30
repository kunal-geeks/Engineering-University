# Exercise Solutions — Lesson 8

## 1–2. Rectangle and Square

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

    def __repr__(self):
        return f"Rectangle(width={self.width}, height={self.height})"


class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)
```

## 3. Book dataclass

```python
from dataclasses import dataclass

@dataclass
class Book:
    title: str
    author: str
    pages: int
```

## 4. __repr__ on Rectangle

Included in the solution above.
