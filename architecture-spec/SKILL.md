---
name: "architecture-spec"
description: "Use only at workspace root in a multi-service repo with authoritative `ARCHITECTURE.md` when deciding cross-repo ownership, compatibility, or service-to-service contracts. Do not use for service-local `SPEC.md` work."
---

# Architecture Spec

Use this skill only when all of the following are true:
- you are at the root of a workspace with many service repositories beneath it
- that root contains an authoritative `ARCHITECTURE.md`
- the task is about cross-service ownership, compatibility, boundaries, or contracts

Do not use this skill for:
- single-repository work
- service-local implementation details
- child `SPEC.md` edits that do not change cross-service rules

## Purpose

`ARCHITECTURE.md` is the centralized cross-service standard.

It should define:
- service responsibilities and boundaries
- ownership of shared domains
- canonical authority for cross-service data
- service-to-service contract patterns
- shared invariants and interaction rules
- shared finite enums or keys only when multiple services must agree on them

It should not define:
- service-local route details
- one service's DTO field inventory
- page fields or UI-only behavior
- persistence details owned by one service
- implementation-local code structure

## Required Workflow

Before editing `ARCHITECTURE.md`:
1. read the root `AGENTS.md`
2. read the root `ADR.md` if present
3. read the current `ARCHITECTURE.md`
4. identify the owning child `SPEC.md` files for the affected domains
5. read the affected child repo `AGENTS.md` files before proposing cross-repo changes

## Core Rules

- Keep `ARCHITECTURE.md` focused on cross-repository rules only.
- Child `SPEC.md` files must conform to `ARCHITECTURE.md`.
- `ARCHITECTURE.md` enumerates shared domains and ownership boundaries.
- The owning child `SPEC.md` defines the exact concrete contract.
- Consumer `SPEC.md` files describe usage and implementation, not alternate ownership.
- Every cross-service datum must have one authority.
- Do not preserve compatibility shims when the architecture has already moved forward.
- The owning service is the default source of truth for shared contracts and shared product state interactions.
- Services consuming shared contracts must use the owning service’s published client package from its package/module path.
- Do not use sibling-directory imports, relative filesystem imports, copied OpenAPI files, or duplicate hand-rolled shared API DTOs in consumers.
- If an API client package changes, every consuming service must update from the published package and verify compatibility.
- Frontend caches and derived UI state are UX/performance layers only; they must not override API-originated authoritative values.
- Do not change backend API behavior ad hoc to satisfy frontend validation; escalate missing or non-conforming backend behavior through the intended service contract.
- Before changing a second service outside the current request, explain the coordination need and get approval to update that service's `AGENTS.md` when appropriate.

## Ownership Pattern

Use this split:
1. enumerate in `ARCHITECTURE.md`
2. define concretely in the owning child `SPEC.md`
3. implement in consuming services

For shared API-owned domains:
- `ARCHITECTURE.md` states the owner and invariant
- the owning service `SPEC.md` defines the exact object model and contract
- consuming services must not redefine that domain incompatibly

## Editing Test

Before adding something to `ARCHITECTURE.md`, ask:
- does this coordinate multiple services?
- does this define ownership or authority?
- does this establish a shared invariant or contract boundary?

If no, put it in the owning child `SPEC.md` instead.

## Drift Resolution

When `ARCHITECTURE.md` and child `SPEC.md` files drift:
- keep the cross-service rule in `ARCHITECTURE.md`
- move concrete service-local detail into the owning child `SPEC.md`
- remove duplicate detail from `ARCHITECTURE.md`
- ensure all affected child `SPEC.md` files align afterward

## Writing Style

- write short declarative rules
- prefer `owns`, `defines`, `consumes`, `derives`, `must`, and `must not`
- avoid long narrative explanation
- avoid duplicating the same rule in multiple sections
- favor minimal examples only when they clarify ownership

## Output Expectations

When changing `ARCHITECTURE.md`, report:
- the cross-service rule added or changed
- the owning service
- the consuming services
- which child `SPEC.md` files must align
- whether follow-on cross-repo coordination is required
