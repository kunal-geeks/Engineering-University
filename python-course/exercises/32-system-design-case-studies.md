# Exercise Solutions — Lesson 32

## URL shortener (sketch)

- **API:** POST /links `{url}` → `{short_code}`; GET /{code} → 302
- **Storage:** Postgres (code PK, url, created_at) + Redis cache hot codes
- **Scale:** base62 code generation; read-heavy → cache + CDN for redirects
- **Analytics:** async click events to queue → OLAP

## Real-time leaderboard

- **Fast path:** Redis sorted set (`ZINCRBY`)
- **Consistency:** periodic sync to Postgres; accept seconds lag for display
- **Tradeoff:** strong consistency not required for gamification UX
