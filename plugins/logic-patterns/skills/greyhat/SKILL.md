---
name: "greyhat"
description: "Use for an adversarial security review loop focused on vulnerabilities, compliance, data safety, intrusion resistance, and endpoint hardening before implementation closes."
---

# Greyhat

Use this skill when security, compliance, or abuse-resistance concerns must be reviewed adversarially and repeatedly until risk is reduced.

## Purpose

Run a structured adversary loop to uncover hidden risks in security-critical behavior, then fix only the highest-risk issues first.

## Review workflow

1. Define scope, trust boundaries, and required controls
   - data sensitivity, blast radius, threat actor model, and compliance obligations.
2. Build explicit adversary hypotheses
   - attack each surface as if it is actively targeted.
3. Run adversarial pass
   - review code, configuration, deployment path, and integrations.
4. Rank findings by severity and exploitability
   - focus first on high-confidence, high-impact issues.
5. Patch and validate
   - apply minimal safe fixes.
   - add or update focused security tests/checks.
6. Re-run the loop
   - repeat until only low-risk, accepted residual risk remains.

## Security review topics (enumerate before changes)

1. Authentication bypass
2. Authorization mismatch
3. Session fixation and token theft risk
4. Secret and credential exposure
5. Encryption at rest and in transit
6. Key management and rotation
7. Input validation gaps
8. SQL/NoSQL/graph query injection
9. Command injection and shell usage
10. SSRF and outbound network abuse
11. Broken access controls
12. Endpoint authentication hardening
13. Rate limiting and abuse throttling
14. Replay and CSRF protections
15. Log injection and sensitive log data
16. Data exfiltration paths
17. Least privilege in IAM/permissions
18. Dependency supply-chain risk
19. Webhook/inbound integration abuse
20. Incident response and rollback hooks

## Closing condition

- No unresolved high/critical security or compliance findings.
- Remaining risks are documented with explicit owner, mitigation, and acceptance reason.

## Report format

- confirmed vulnerabilities and exact fix locations
- severity, confidence, and exploit path
- validation commands/checks executed
- residual risk and monitoring signal to watch
