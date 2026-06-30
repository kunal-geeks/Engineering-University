# Lesson 33: Architecture Governance

Principal engineers don't just design systems — they create the processes and standards that keep dozens of engineers aligned without bottlenecks.

## Architecture Decision Records (ADRs)

Lightweight, version-controlled decision log:

```markdown
# ADR-0042: Use Redis for session storage

## Status
Accepted

## Context
Sessions stored in Postgres cause write load on auth path.

## Decision
Store sessions in Redis with 24h TTL.

## Consequences
+ Lower DB load, faster reads
- New dependency; session loss if Redis fails (acceptable — re-login)
- Requires Redis HA in production
```

Store in `docs/adr/` — immutable history; supersede with new ADRs.

## RFC Process

For larger changes:

1. **Problem statement** — user/business impact
2. **Proposed solution** — diagrams, APIs, migration
3. **Alternatives considered**
4. **Rollout plan** — phases, feature flags, rollback
5. **Review period** — async comments, office hours
6. **Decision** — owner, date, stakeholders

RFCs scale decision-making beyond one architect.

## Technical Standards

Document org-wide defaults:

```markdown
# Python Service Standard v2

- Python 3.11+, src layout, pyproject.toml
- FastAPI for HTTP; SQLAlchemy 2.0 for ORM
- structlog JSON logs + OpenTelemetry traces
- mypy --strict in CI
- Minimum 80% coverage on domain layer
```

Standards reduce bike-shedding; include **escape hatch** process for exceptions.

## Deprecation Policy

```
Announce → Warn (logs/metrics) → Migrate → Remove
Minimum 2 release cycles or 90 days
```

Track usage of deprecated APIs; don't remove until metrics show zero.

## Dependency Governance

- Approved package list or automated scanning
- Quarterly dependency upgrade sprints
- No unmaintained packages (check last PyPI release)

## Design Review Cadence

| Change size | Review |
|-------------|--------|
| Local refactor | PR only |
| New service | RFC + arch review |
| Cross-team contract | RFC + affected teams |
| Data model breaking | RFC + migration plan |

Principal engineers facilitate — they don't block every PR.

## Documentation Hierarchy

```
README (onboard in 15 min)
  → Runbooks (operate in incident)
    → ADRs (why we built it this way)
      → RFCs (major future changes)
```

## Key Takeaways

- ADRs capture context future you will forget.
- RFCs scale decisions; standards reduce repeated debates.
- Deprecation is a product — communicate and measure adoption.
- Governance enables speed through clarity, not gatekeeping.

## Exercises

1. Write an ADR for a caching technology choice in a hypothetical API.
2. Draft a one-page Python service standard for a 20-engineer org.
3. Create deprecation plan for a v1 API endpoint with timeline.
4. Define which changes require RFC vs PR-only review.

## Solutions

See [exercises/33-architecture-governance.md](../exercises/33-architecture-governance.md)

## Next

[Lesson 34: Code Review at Scale →](34-code-review-at-scale.md)
