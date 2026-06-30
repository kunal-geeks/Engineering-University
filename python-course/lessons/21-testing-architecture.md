# Lesson 21: Testing Architecture

Principal engineers design test systems that enable velocity — fast feedback, meaningful coverage, and contracts that survive refactors.

## Test Pyramid

```
        /\
       /  \  E2E (few)
      /----\
     /      \ Integration (some)
    /--------\
   /          \ Unit (many)
  /--------------\
```

- **Unit** — pure functions, domain logic, milliseconds
- **Integration** — DB, HTTP, message queues with real or containerized deps
- **E2E** — critical user journeys only

## pytest Fundamentals

```python
# test_math.py
import pytest

def add(a, b):
    return a + b

def test_add():
    assert add(2, 3) == 5

@pytest.mark.parametrize("a,b,expected", [(0, 0, 0), (-1, 1, 0)])
def test_add_cases(a, b, expected):
    assert add(a, b) == expected
```

## Fixtures — Setup Reuse

```python
import pytest

@pytest.fixture
def db_session():
    session = create_test_session()
    yield session
    session.rollback()
    session.close()

def test_create_user(db_session):
    user = User(name="Ada")
    db_session.add(user)
    db_session.commit()
    assert user.id is not None
```

### Fixture scopes

`function` (default), `class`, `module`, `session` — wider scope = faster but more isolation risk.

## conftest.py

Shared fixtures live in `tests/conftest.py` automatically discovered by pytest.

## Mocking — unittest.mock

```python
from unittest.mock import AsyncMock, patch

@patch("myapp.email.send")
def test_signup_sends_email(mock_send):
    signup("ada@example.com")
    mock_send.assert_called_once()

@pytest.mark.asyncio
async def test_async_service():
    mock_client = AsyncMock()
    mock_client.get.return_value.status_code = 200
    ...
```

Mock **boundaries**, not internals. Over-mocking couples tests to implementation.

## Factory Pattern for Test Data

```python
def make_user(**overrides):
    defaults = {"name": "Test", "email": "t@example.com"}
    return User(**{**defaults, **overrides})
```

Consider `factory_boy` for complex models.

## Property-Based Testing (Hypothesis)

```python
from hypothesis import given, strategies as st

@given(st.integers(), st.integers())
def test_add_commutative(a, b):
    assert add(a, b) == add(b, a)
```

Finds edge cases humans miss — overflow, empty strings, unicode.

## Contract Tests

When service A calls service B:

```python
def test_api_contract_matches_consumer_expectations():
    response = client.get("/users/1")
    assert response.status_code == 200
    assert "id" in response.json()
    assert "email" in response.json()
```

Use Pact or schema validation (JSON Schema, OpenAPI) for cross-team contracts.

## Test Markers and CI

```python
@pytest.mark.slow
@pytest.mark.integration
def test_full_pipeline(): ...
```

```bash
pytest -m "not slow"          # PR pipeline
pytest -m integration         # nightly
```

## Coverage — Use Wisely

```bash
pytest --cov=src --cov-report=term-missing
```

100% coverage ≠ correctness. Focus on critical paths and branch coverage for domain logic.

## Async Testing

```bash
pip install pytest-asyncio
```

```python
@pytest.mark.asyncio
async def test_fetch():
    result = await fetch_user("1")
    assert result["id"] == "1"
```

## Key Takeaways

- Design for testability: inject dependencies, pure domain logic.
- Fixtures and factories reduce duplication; mocks at system boundaries.
- Property-based tests complement example-based tests.
- CI runs fast unit tests on every PR; integration tests on schedule.

## Exercises

1. Refactor a function to accept a `Notifier` Protocol; test with a fake.
2. Add parametrized tests covering edge cases for a parsing function.
3. Write one Hypothesis test for a sorting or encoding function.
4. Define a test strategy document: what runs on PR vs nightly vs release.

## Solutions

See [exercises/21-testing-architecture.md](../exercises/21-testing-architecture.md).

## Next

[Lesson 22: Packaging & Build Systems →](22-packaging-and-build-systems.md)
