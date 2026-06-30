# Lesson 26: Security Engineering

Security is not a checklist at the end — it is woven into API design, dependencies, and operations. Principal engineers own threat modeling for their domains.

## OWASP Top Risks (Python Context)

| Risk | Mitigation |
|------|------------|
| Injection | Parameterized queries (SQLAlchemy), never `f"SELECT ... {user_input}"` |
| Broken auth | OAuth2/JWT best practices, short-lived tokens, secure cookies |
| Sensitive data exposure | Encrypt at rest, TLS in transit, no secrets in logs |
| SSRF | Allowlist outbound URLs, validate user-supplied hosts |
| Vulnerable components | Dependabot, `pip-audit`, pinned deps |

## Authentication Patterns

```python
from fastapi import Depends, HTTPException
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
import jwt

security = HTTPBearer()

def get_current_user(creds: HTTPAuthorizationCredentials = Depends(security)):
    try:
        payload = jwt.decode(creds.credentials, SECRET, algorithms=["HS256"])
        return payload["sub"]
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401)
```

Prefer industry libraries (`authlib`, identity providers) over rolling crypto.

## Secrets Management

```python
# Never
API_KEY = "sk-live-abc123"

# Better
import os
API_KEY = os.environ["API_KEY"]

# Production
# AWS Secrets Manager, GCP Secret Manager, HashiCorp Vault
```

Rotate secrets without redeploying app code when possible.

## Password Hashing

```python
from passlib.context import CryptContext

pwd = CryptContext(schemes=["bcrypt"], deprecated="auto")
hash = pwd.hash("user-password")
pwd.verify("user-password", hash)
```

Never MD5/SHA1 for passwords.

## Input Validation

Pydantic validates types and constraints at the boundary:

```python
class CreatePost(BaseModel):
    title: str = Field(max_length=200)
    body: str = Field(max_length=50_000)
```

Validate **again** in domain rules where business constraints apply.

## SQL Injection — Always Parameterized

```python
# Vulnerable
session.execute(text(f"SELECT * FROM users WHERE email = '{email}'"))

# Safe
session.execute(select(User).where(User.email == email))
```

## Supply Chain Security

```bash
pip install pip-audit
pip-audit
```

- Pin dependencies in applications
- Review new packages (maintainer, downloads, source)
- Use private PyPI mirror in enterprises

## CORS, CSRF, Rate Limiting

```python
from slowapi import Limiter
limiter = Limiter(key_func=get_remote_address)

@app.post("/login")
@limiter.limit("5/minute")
async def login(): ...
```

Configure CORS explicitly — never `allow_origins=["*"]` with credentials.

## Security Headers

Use middleware for `Strict-Transport-Security`, `X-Content-Type-Options`, `Content-Security-Policy`.

## Key Takeaways

- Parameterized queries, validated inputs, hashed secrets.
- No credentials in code or logs; use secret managers.
- Audit dependencies continuously; respond to CVEs with SLA.
- Threat model new features before implementation.

## Exercises

1. Run `pip-audit` on a project; triage and fix one finding.
2. Add rate limiting to an auth endpoint.
3. Write a threat model (STRIDE) for a file upload feature.
4. Document secret rotation procedure for your API keys.

## Solutions

See [exercises/26-security-engineering.md](../exercises/26-security-engineering.md)

## Next

[Lesson 27: Caching & Background Jobs →](27-caching-and-background-jobs.md)
