# Exercise Solutions — Lesson 24

## Pool sizing (4 Uvicorn workers)

Rule of thumb: `pool_size ≈ workers × 2`, `max_overflow ≈ workers × 2` as starting point — tune from APM wait times and Postgres `max_connections`. Example: `pool_size=8`, `max_overflow=8`, monitor connection checkout latency.

## N+1 fix

```python
from sqlalchemy.orm import selectinload

stmt = select(Order).options(selectinload(Order.items))
orders = session.scalars(stmt).all()
```

## Backward-compatible migration

```python
# alembic revision
def upgrade():
    op.add_column("users", sa.Column("nickname", sa.String(), nullable=True))

def downgrade():
    op.drop_column("users", "nickname")
```

Deploy: migrate first (nullable column), then deploy app code that writes nickname, then backfill, then add NOT NULL in later migration if needed.
