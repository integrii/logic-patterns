---
name: "spec-driven-development"
description: "Use when creating or updating a repository-local `SPEC.md` that defines desired-state behavior, contracts, and implementation constraints for that repo. Not for cross-repo architecture work."
---

# Spec Driven Development

Use this skill when a repository should have a local `SPEC.md` that defines what the repository is supposed to build.

Use the existing child `SPEC.md` files under this workspace as the style baseline:
- `api-service/SPEC.md`
- `frontend/SPEC.md`
- `gateway-service/SPEC.md`

Do not use this skill for:
- root cross-repository architecture work governed by `ARCHITECTURE.md`
- implementation-only edits that do not affect the repository specification
- ad hoc notes, plans, or temporary design docs

## Purpose

`SPEC.md` is the desired-state engineering specification for one repository.

It should describe, succinctly:
- repository purpose and scope
- service responsibilities and non-goals
- object model and key domain structures
- areas of concern and ownership boundaries inside the repo
- key implementation rules and invariants
- API surface area and concrete contract expectations
- page-specific UI elements and route behavior when the repo owns UI
- handler inventory or route-family inventory when relevant
- key packages other repos are expected to import
- repository-specific decisions that materially affect implementation

`SPEC.md` is not:
- a changelog
- a task plan
- a copy of code comments
- a duplicate of root `ARCHITECTURE.md`
- a dump of every implementation detail in the repo

## Required Workflow

Before changing `SPEC.md`:
1. read the repo-local `AGENTS.md`
2. read the repo-local `ADR.md` if present
3. read the root `ARCHITECTURE.md` when cross-service boundaries matter
4. read the current `SPEC.md`
5. inspect the relevant code, handlers, DTOs, packages, and tests

## Core Rules

- Treat `SPEC.md` as desired-state, not current-state commentary.
- Treat any `SPEC.md` under `.worktrees/` as non-authoritative unless the active work is explicitly in that worktree.
- When code and `SPEC.md` differ, move code toward `SPEC.md` unless the spec itself is wrong.
- Keep `SPEC.md` repository-local; do not let it redefine cross-repo ownership already defined in `ARCHITECTURE.md`.
- Exact shared contracts belong in the owning repo's `SPEC.md`.
- Consumer repos may describe usage of a shared contract, but must not become alternate authorities for it.
- Prefer one canonical contract shape for each supported behavior.
- Do not preserve legacy names, aliases, or compatibility shims in the spec once the desired state is clear.
- Every contract-served, rendered, emitted, stored, calculated, or operator-visible datum must have exactly one authoritative origin.
- Fallback handling is banned for authoritative data: no alternate endpoint reads, competing field merges, stale cache replay, unrelated-payload inference, or hardcoded substitution.
- If the authoritative fetch, decode, derive, or calculation path fails, fail explicitly according to the owning contract.
- Caches are transport/performance layers only; they must not override or replace the authoritative origin.
- Known persisted shapes must use typed relational tables and columns; avoid Postgres blobs for known structure.
- Finite domains such as status, type, mode, state, reason, level, phase, event kind, provider, and action key should use concrete named enum types.
- Avoid schema-less maps or blank interfaces for known payloads.
- Upstream SDK models may be used when they are the authoritative third-party contract, even if they contain open-ended internals.
- If a third-party contract is genuinely open-ended, document that exception in the owning `SPEC.md`.
- Do not use "forward-only" as permission to drop desired functionality.
- Before deleting or renaming code, prove it is retired compatibility, unreachable, or superseded by the desired-state spec.
- For refactors and simplifications, identify touched behavioral surfaces such as routes, DTOs, storage rows, events, commands, manifests, and browser flows.
- Use LSP, `rg`, focused tests, and route/contract coverage to verify references and behavior before removing surfaces.
- Preserve externally observable behavior unless the active request, `ARCHITECTURE.md`, `SPEC.md`, or `ADR.md` requires a desired-state change.

## What Good `SPEC.md` Sections Usually Cover

- purpose and scope
- responsibilities and non-goals
- architectural decisions or repository rules
- object model / important structures
- handler and route ownership
- API or stream contract rules
- package responsibilities
- UI route/page contracts when the repo owns UI
- typing and enum rules when important
- testing and verification expectations when they materially shape implementation

Not every repo needs every section, but every repo should have enough specification to make ownership, contracts, and intended behavior clear.

## Writing Style

- write short declarative rules
- prefer `must`, `must not`, `owns`, `defines`, `returns`, `accepts`, `requires`
- organize by concern, not by code file order
- keep examples brief
- avoid restating the same rule in multiple sections
- keep the spec dense with decisions, not prose

## Drift Resolution

When `SPEC.md` drifts:
- keep cross-repo rules in `ARCHITECTURE.md`
- keep repo-local concrete behavior in `SPEC.md`
- remove duplicated material instead of maintaining two copies
- align handlers, SDK/contracts, tests, and docs in the same change when possible

## Output Expectations

When updating `SPEC.md`, report:
- the repository concern clarified or changed
- the affected contract, handler family, object model, package, or page surface
- whether any sibling repo specs or the root `ARCHITECTURE.md` must also change
