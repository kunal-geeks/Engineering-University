# Exercise Solutions — Lesson 34

## PR template

```markdown
## Summary
<!-- Why this change? -->

## Test plan
- [ ] Unit tests
- [ ] Manual verification steps

## Rollout / rollback
<!-- Feature flags, migrations, risks -->

## Links
<!-- Ticket, RFC, ADR -->
```

## CI merge gates

- ruff check + format
- mypy --strict
- pytest (unit)
- pip-audit (no high CVEs)
- 1 approving review; 2 for auth/payment paths
