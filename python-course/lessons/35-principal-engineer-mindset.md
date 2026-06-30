# Lesson 35: Principal Engineer Mindset

Principal engineer is not "senior who writes more code." It is scope across teams, long time horizons, and accountability for technical outcomes — often without direct authority.

## Scope Expansion

| Level | Typical scope |
|-------|---------------|
| Senior | Feature / service |
| Staff | Multi-service / platform area |
| Principal | Org-wide architecture, critical paths, tech strategy |

You influence through clarity, prototypes, and trust — not title alone.

## Technical Strategy

Connect engineering work to business outcomes:

```
Company goal: reduce churn
  → Product: faster checkout
    → Engineering: checkout latency SLO, payment reliability
      → Projects: cache catalog, idempotent payments, circuit breakers
```

Principal engineers articulate this chain and prioritize accordingly.

## Tradeoff Framework

Every significant decision has costs:

```markdown
## Option A: Modular monolith
+ Faster delivery, simpler ops
- Scaling team autonomy harder at 50+ engineers

## Option B: Microservices now
+ Team independence
- Operational overhead we can't staff yet

## Recommendation
Monolith with clear module boundaries; extract billing at Q3 if team splits.
```

Document dissent — good decisions survive scrutiny.

## Navigating Ambiguity

When requirements are unclear:

1. Clarify the **problem** before the solution
2. Ship smallest learning experiment (prototype, spike ADR)
3. Set **time-box** — don't analysis-paralyze
4. Communicate risks explicitly to stakeholders

## Cross-Team Leadership

- Run design reviews, don't dominate them
- Unblock others — remove systemic friction (CI slowness, missing docs)
- Represent engineering in product tradeoff discussions with data
- Mentor staff/senior engineers into leaders

## When to Deep Dive vs Delegate

| Situation | Principal action |
|-----------|------------------|
| Novel architectural risk | Lead design |
| Repeated incident class | Build platform fix |
| One-off bug | Delegate; review postmortem actions |
| Team conflict on approach | Facilitate ADR/RFC |

Your leverage is multiplication, not heroics.

## Communication Artifacts

Principal engineers write constantly:

- RFCs and ADRs
- Strategy docs (1–2 pages, executive summary first)
- Runbooks and onboarding guides
- Postmortems and reliability reports

Writing forces clarity; async orgs run on documents.

## Saying No (Constructively)

```
"We shouldn't build X now because:
 1. Error budget exhausted this quarter
 2. Alternative Y achieves 80% value at 20% cost
 3. Proposed timeline ignores migration risk

I recommend Y with milestone review in 6 weeks."
```

## Staying Technical

Principal ≠ manager. Stay credible by:

- Reading code in critical paths monthly
- Prototyping risky ideas yourself
- Reviewing security and reliability changes
- Tracking ecosystem shifts (Python release cadence, async drivers, typing tools)

## Key Takeaways

- Scope is organizational impact, not LOC.
- Strategy links tech choices to business outcomes.
- Influence through documents, prototypes, and mentorship.
- Protect focus — depth on problems that matter at scale.

## Exercises

1. Write a one-page tech strategy for a hypothetical product at Series B stage.
2. Facilitate a mock ADR discussion — document two options and a recommendation.
3. Identify one systemic friction on a team and propose a platform fix.
4. Map your current role to senior/staff/principal scope — gap analysis.

## Solutions

See [exercises/35-principal-engineer-mindset.md](../exercises/35-principal-engineer-mindset.md)

## Next

[Lesson 36: Capstone & Career Synthesis →](36-capstone-and-career-synthesis.md)
