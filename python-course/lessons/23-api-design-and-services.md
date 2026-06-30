# Lesson 23: API Design and Services

Well-designed APIs outlive implementations. Principal engineers define boundaries, evolution strategy, and consistency across services.

## Layered Service Architecture

```
HTTP Layer (routes, auth, serialization)
    ↓
Application Layer (use cases, orchestration)
    ↓
Domain Layer (business rules, pure logic)
    ↓
Infrastructure (DB, queues, external APIs)
```

Keep domain logic free of FastAPI/Flask imports.

## FastAPI + Pydantic v2

```python
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel, EmailStr, Field

app = FastAPI(title="Users API", version="1.0.0")

class CreateUserRequest(BaseModel):
    email: EmailStr
    name: str = Field(min_length=1, max_length=100)

class UserResponse(BaseModel):
    id: str
    email: EmailStr
    name: str

@app.post("/v1/users", response_model=UserResponse, status_code=201)
async def create_user(body: CreateUserRequest, svc: UserService = Depends(get_user_service)):
    try:
        user = await svc.create(body.email, body.name)
    except DuplicateEmailError:
        raise HTTPException(status_code=409, detail="email already exists")
    return UserResponse.model_validate(user)
```

## Dependency Injection

```python
async def get_db():
    async with session_factory() as session:
        yield session

async def get_user_service(db=Depends(get_db)):
    return UserService(UserRepository(db))
```

Test by overriding dependencies:

```python
app.dependency_overrides[get_user_service] = lambda: FakeUserService()
```

## Pagination

Cursor-based (preferred at scale):

```python
class Page(BaseModel):
    items: list[UserResponse]
    next_cursor: str | None

@app.get("/v1/users")
async def list_users(cursor: str | None = None, limit: int = 50):
    ...
```

Offset pagination degrades on large tables (`OFFSET` scans).

## Versioning Strategy

| Strategy | Example | Tradeoff |
|----------|---------|----------|
| URL path | `/v1/users` | Explicit, easy routing |
| Header | `Accept-Version: 1` | Clean URLs, harder caching |
| None (expand-only) | Add optional fields | Requires discipline |

Document deprecation timelines (e.g. 6 months notice).

## Error Response Contract

```python
class ErrorBody(BaseModel):
    code: str
    message: str
    details: dict | None = None
```

Use consistent codes: `USER_NOT_FOUND`, `VALIDATION_ERROR` — not raw stack traces to clients.

## Idempotency Keys

For POST that create resources:

```python
@app.post("/v1/payments")
async def create_payment(
    body: PaymentRequest,
    idempotency_key: str = Header(..., alias="Idempotency-Key"),
):
    ...
```

Store key → response mapping; return same response on retry.

## OpenAPI as Contract

FastAPI generates OpenAPI — use it for:

- Client SDK generation
- Contract tests
- API documentation

## Key Takeaways

- Separate HTTP, application, domain, and infrastructure layers.
- Pydantic models at boundaries; domain types internally.
- Cursor pagination, idempotency, and consistent errors are production requirements.
- Version and deprecate APIs with written policy.

## Exercises

1. Build a CRUD API with repository pattern and dependency injection.
2. Add cursor pagination and document the cursor format.
3. Implement idempotency for a create endpoint with Redis backing.
4. Write an ADR choosing URL versioning for your API.

## Solutions

See [exercises/23-api-design-and-services.md](../exercises/23-api-design-and-services.md).

## Next

[Lesson 24: Data Layer Patterns →](24-data-layer-patterns.md)
