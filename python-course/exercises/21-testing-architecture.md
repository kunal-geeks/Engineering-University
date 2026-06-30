# Exercise Solutions — Lesson 21

## 1. Notifier Protocol + fake

```python
class FakeNotifier:
    def __init__(self):
        self.sent = []

    def send(self, user_id: str, message: str) -> None:
        self.sent.append((user_id, message))

def test_signup_notifies():
    notifier = FakeNotifier()
    signup("u1", notifier=notifier)
    assert notifier.sent == [("u1", "Welcome!")]
```

## 3. Hypothesis example

```python
from hypothesis import given, strategies as st

@given(st.text())
def test_reverse_twice_is_identity(s):
    assert s[::-1][::-1] == s
```

## 4. Test strategy doc

| Suite | When | Target |
|-------|------|--------|
| Unit | Every PR | < 2 min |
| Integration | Nightly | DB/Redis containers |
| E2E | Pre-release | Critical paths |
| Load | Weekly staging | SLO budgets |
