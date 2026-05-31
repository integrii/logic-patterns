---
name: "architecture-decision-records"
description: "Use when adding, updating, or consulting repository-local `ADR.md` architecture decisions. Keep ADR authority user-owned and ensure planning flows load `./ADR.md` via `AGENTS.md`."
---

# ADR Recordkeeping

Use repository-local `./ADR.md` as the source of truth for user-decided architecture and system design decisions.

Rules:
- when starting work in a new repository, create or locate `./ADR.md`
- before architecture or design decisions, load and consult `./ADR.md`
- record only decisions the user explicitly makes
- do not record your own preferences or inferred architecture choices as ADR decisions
- if a repository has `AGENTS.md`, ensure it tells agents to load `./ADR.md` before creating a development plan
- treat `ADR.md` as repository-local accepted decision authority; keep cross-repository rules in root `ARCHITECTURE.md` and service-local desired behavior in `SPEC.md`

Good ADR content:
- rendering model
- deployment stack
- language constraints
- explicit user rules such as `always SSR` or `deploy via Kargo`

Do not use `ADR.md` as a dump for incidental implementation notes.
