# Lesson 36: Capstone and Career Synthesis

This final lesson integrates the course into demonstrable principal-level work and a plan for continued growth.

## Principal-Level Portfolio Projects

Choose one that spans **multiple course areas** and produce public artifacts (blog, talk, or repo):

### Option A: Production-Grade Micro-Platform

Build a small but complete platform:

```
api/          — FastAPI, layered architecture, OpenAPI
worker/       — ARQ/Celery, idempotent jobs, DLQ
shared/       — domain models, typed protocols
infra/        — Docker Compose: Postgres, Redis, Prometheus
docs/         — ADRs, runbooks, architecture diagram
```

Demonstrates: Parts II–IV.

### Option B: Reliability Case Study

Take an existing open-source Python service:

- Add structured logging + OpenTelemetry
- Define SLOs and Grafana dashboards
- Write postmortem for a simulated failure
- Publish improvement RFC

### Option C: Library Used by Others

Publish a focused PyPI library:

- Strict typing, 95%+ test coverage
- Semantic versioning, changelog
- Documentation site
- Respond to issues — maintainership experience

## Interview Preparation (Principal Loop)

Expect:

| Round | Focus |
|-------|-------|
| Coding | Python fluency, clean design — not LeetCode trivia |
| System design | End-to-end with tradeoffs, operability |
| Architecture | Past decisions, failures, scaling stories |
| Leadership | Conflict, mentorship, influencing without authority |

Prepare **STAR stories** with metrics:

- "Reduced p95 latency 40% by..."
- "Led migration of 12 services to..."
- "Prevented outage by adding circuit breaker after postmortem..."

## Competency Self-Assessment

Revisit the [README competency rubric](../README.md#competency-rubric). Rate yourself 1–5 on each row. Gaps become your learning plan.

## Continued Learning

| Area | Resources |
|------|-----------|
| Language depth | *Fluent Python* (Ramalho) |
| Architecture | *Architecture Patterns with Python* (Percival & Gregory) |
| Distributed systems | *Designing Data-Intensive Applications* (Kleppmann) |
| Python releases | What's New in Python 3.12, 3.13 docs |
| Community | PyCon talks, local meetups, OSS contributions |

## Teaching Others

Principal engineers grow the org:

- Internal workshops on asyncio, testing, or observability
- Mentorship pairings with explicit goals
- Lunch talks on postmortems (learning culture)

Teaching deepens your own mastery.

## Your 90-Day Plan Template

```markdown
## Goals (pick 2–3)
- [ ] Ship capstone project to staging with SLO dashboard
- [ ] Write 3 ADRs for current team decisions
- [ ] Lead one RFC end-to-end

## Weekly habits
- 2h deep reading (books, PEPs, postmortems)
- Review one production incident (internal or public writeup)
- One PR review focused on mentorship

## Success metrics
- Capstone deployed with documented runbook
- Present architecture to team/leads
- Measurable improvement in one reliability or velocity metric
```

## Course Complete

You have traveled from first script to the technical depth and organizational scope of a **principal Python engineer**:

- **Part I** — idiomatic foundations
- **Part II** — language and runtime mastery
- **Part III** — production systems at scale
- **Part IV** — reliability, architecture, and leadership

The path continues in production — build, break, measure, document, teach.

## Final Exercises

1. Complete one capstone option with README, ADRs, and runbook.
2. Fill the 90-day plan with specific goals for your context.
3. Present a 15-minute architecture walkthrough of your capstone (record or slides).
4. Write a "lessons learned" doc — what you'd do differently on a second iteration.

## Solutions

See [exercises/36-capstone-and-career-synthesis.md](../exercises/36-capstone-and-career-synthesis.md)
