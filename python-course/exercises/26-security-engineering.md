# Exercise Solutions — Lesson 26

## STRIDE threat model — file upload

| Threat | Mitigation |
|--------|------------|
| Spoofing | Auth required |
| Tampering | Virus scan, hash verification |
| Repudiation | Audit log of uploads |
| Info disclosure | Private bucket, signed URLs |
| DoS | Size limits, rate limits |
| Elevation | Validate MIME/extension; store outside web root |

## Secret rotation procedure

1. Generate new key in secret manager
2. Deploy app reading both old and new keys (validation window)
3. Update clients to new key
4. Revoke old key after metrics show zero old-key usage
5. Document in runbook with rollback steps
