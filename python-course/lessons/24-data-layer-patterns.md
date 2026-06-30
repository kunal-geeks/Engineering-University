# Lesson 24: Data Layer Patterns

Data is where most production incidents originate. Principal engineers design repositories, transactions, and migrations that preserve integrity under failure and scale.

## SQLAlchemy 2.0 Style

```python
from sqlalchemy import select
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, Session

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"
    id: Mapped[int] = mapped_column(primary_key=True)
    email: Mapped[str] = mapped_column(unique=True)
    name: Mapped[str]

def get_user_by_email(session: Session, email: str) -> User | None:
    stmt = select(User).where(User.email == email)
    return session.scalar(stmt)
```

Prefer 2.0 `select()` over legacy `session.query()`.

## Repository Pattern

```python
class UserRepository:
    def __init__(self, session: Session):
        self.session = session

    def get(self, user_id: int) -> User | None:
        return self.session.get(User, user_id)

    def add(self, user: User) -> User:
        self.session.add(user)
        return user
```

Repositories isolate persistence from domain services — swap implementations in tests.

## Unit of Work / Transactions

```python
from contextlib import contextmanager

@contextmanager
def unit_of_work(session_factory):
    session = session_factory()
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()

with unit_of_work(SessionLocal) as session:
    repo = UserRepository(session)
    repo.add(User(email="a@b.com", name="Ada"))
```

One transaction per use case — avoid long-lived sessions.

## N+1 Query Problem

```python
# Bad — N+1
users = session.scalars(select(User)).all()
for u in users:
    print(u.orders)  # lazy load per user

# Good — eager load
from sqlalchemy.orm import selectinload
stmt = select(User).options(selectinload(User.orders))
```

Detect with SQL logging or APM query counts.

## Migrations (Alembic)

```bash
alembic init migrations
alembic revision --autogenerate -m "add users table"
alembic upgrade head
```

Rules:

- Never edit applied migrations in production
- Backward-compatible migrations for zero-downtime deploys (add column nullable first)
- Test rollback paths

## Connection Pooling

```python
engine = create_engine(
    DATABASE_URL,
    pool_size=10,
    max_overflow=20,
    pool_pre_ping=True,  # detect stale connections
)
```

Size pools from: `(workers × concurrent requests × queries per request)`.

## Read Replicas

Route read-heavy queries to replicas; writes to primary. Handle replication lag in UX (eventual consistency messaging).

## Key Takeaways

- Repository + unit of work decouple domain from SQLAlchemy.
- One transaction per request/use case; commit at boundary.
- Eliminate N+1; size connection pools; use `pool_pre_ping`.
- Migrations are code — review them like application changes.

## Exercises

1. Implement UserRepository with get/add/list and in-memory fake for tests.
2. Fix an N+1 scenario using `selectinload` or `joinedload`.
3. Write a backward-compatible Alembic migration adding a nullable column.
4. Document connection pool sizing for a 4-worker FastAPI deployment.

## Solutions

See [exercises/24-data-layer-patterns.md](../exercises/24-data-layer-patterns.md).

## Next

[Lesson 25: Observability →](25-observability.md)
