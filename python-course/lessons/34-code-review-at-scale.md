# Lesson 34: Code Review at Scale

Code review is the primary quality gate in most Python orgs. Principal engineers shape review culture, automation, and mentorship — not just leave nitpick comments.

## Goals of Review

1. **Correctness** — bugs, edge cases, security
2. **Design** — fit with architecture, appropriate abstractions
3. **Operability** — logs, metrics, failure modes
4. **Knowledge transfer** — spread context across team

Not: style debates (automate with ruff) or rewriting to personal preference.

## Author Responsibilities

- Small, focused PRs (< 400 lines changed when possible)
- Clear description: why, what, how to test
- Self-review before requesting review
- Link ticket/RFC/ADR when relevant

```markdown
## Summary
Add idempotency keys to POST /payments

## Test plan
- [ ] Unit tests for duplicate key returns same response
- [ ] Manual: curl with same Idempotency-Key twice

## Rollout
Feature flagged; Redis required in staging
```

## Reviewer Responsibilities

Prioritize feedback:

| Severity | Action |
|----------|--------|
| Blocker | Must fix — security, data loss, broken contract |
| Important | Should fix — design, missing tests |
| Nit | Optional — naming, minor style |
| Question | Understand before judging |

Use prefixes: `[blocker]`, `[suggestion]`, `[nit]`, `[question]`.

## Automation in CI

```yaml
jobs:
  quality:
    steps:
      - run: ruff check .
      - run: ruff format --check .
      - run: mypy --strict src/
      - run: pytest -m "not integration"
      - run: pip-audit
```

Humans review what machines can't: design and business logic.

## Architecture Review in PRs

Ask:

- Does this belong in this bounded context?
- New dependencies justified?
- Migration backward compatible?
- Observability added?
- Failure modes handled?

## Mentorship Through Review

- Explain **why**, not just **what** to change
- Point to docs, ADRs, examples
- Approve with comments for non-blockers — don't stall velocity
- Pair on complex feedback instead of 20 comment rounds

## Review Load at Scale

- CODEOWNERS for domain experts
- Rotate reviewers to spread knowledge
- Senior/principal reviewers on RFC implementations and hot paths only
- Track review latency — bottleneck kills morale

## Anti-Patterns

- Bikeshedding formatting (use formatters)
- Ghost approving without reading
- Perfectionism blocking shipping
- Rubber-stamping security-sensitive changes

## Key Takeaways

- Automate style/types/tests; humans focus on design and risk.
- Tag feedback severity; distinguish blockers from nits.
- Review teaches — principal engineers multiply through others.
- Small PRs get better reviews faster.

## Exercises

1. Write a PR template for your team with test plan and rollout sections.
2. Review a sample PR (real or hypothetical); categorize feedback by severity.
3. Define CI gates for a Python service — what blocks merge?
4. Document CODEOWNERS strategy for a monorepo with three teams.

## Solutions

See [exercises/34-code-review-at-scale.md](../exercises/34-code-review-at-scale.md)

## Next

[Lesson 35: Principal Engineer Mindset →](35-principal-engineer-mindset.md)
