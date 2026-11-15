---
name: "multidimensional-planning"
description: "Use when creating an implementation plan by asking structured questions across orthogonal design vectors so no major planning dimension is missed."
---

# Multidimensional Planning

Use this skill to build a plan before implementation when requirements are broad or the solution is non-trivial.

## Goal

Force complete coverage across independent planning dimensions, then synthesize a single, concrete implementation plan.

## Core Method

1. Start with the objective and success criteria.
2. Process one dimension at a time in the order below.
3. For each dimension:
   - ask the full targeted question set,
   - capture concrete answers,
   - and append those specifics directly into the evolving plan before touching the next dimension.
4. Do not proceed to the next dimension until the current one has at least one concrete, reviewable plan addition.
5. Record unknowns and unresolved decisions in the plan as open action items immediately when found.
6. After all dimensions are processed, collapse them into ordered implementation phases.
7. Re-check for contradictions between dimensions and resolve conflicts before creating implementation tasks.

## Example planning dimensions

Pick any that are useful to the plan, and think of other dimensions to analyze beyond these. For each dimension, ask “what is required?”, “what is optional?”, and “what are the hard constraints?”.

- User outcomes and journeys
  - What are the key user flows?
  - What are success/failure paths?
- Product feature set
  - What are all major user-visible features?
  - What is explicitly out of scope?
- UI and navigation coverage
  - What pages, routes, and route states are required?
  - Which flows are shared vs feature-specific?
- API and contract surface
  - What API surface area is needed and who owns each endpoint?
  - What schemas, auth requirements, and error contracts are required?
- Architecture and boundaries
  - What is the software architecture style (monolith/modules/services)?
  - Where are service boundaries and coupling points?
- Data model and storage
  - What database tables, columns, and index plan are needed?
  - What migration and retention behavior is required?
- Caching and coherence
  - What cache layers exist and where?
  - What invalidation strategy and staleness guarantees are acceptable?
- Package and code ownership
  - What new packages/modules must be created?
  - What dependencies are required or disallowed?
- Frontend surface definition
  - What screens/components are required?
  - What shared UI primitives and state containers are used?
- Observability and operations
  - What logs, traces, and metrics are required?
  - What alerts and runbooks are needed for failures?
- Security and governance
  - What permission model, secret handling, and abuse controls are required?
  - What audit/compliance signals are required?
- Scalability and resilience
  - How will it scale horizontally?
  - What are saturation points and auto-scaling triggers?
- Observability and exposure
  - What observability features (logs, metrics, traces, events, alerts, dashboards) will be exposed?
  - Who consumes each signal and how?
- Logging depth and troubleshootability
  - What logs are required for visibility and future troubleshooting?
  - What log levels, redaction rules, retention expectations, and correlation keys are required?
- Execution model boundaries
  - Which parts can be asynchronous, and which must remain synchronous?
  - What consistency, user-facing latency, and reliability constraints drive each choice?
- Platform topology
  - What unique systems does this project include?
  - How do they coordinate lifecycle and deployment?
- Data locality
  - Where is data authored, cached, transformed, and persisted?
  - What latency/copy and residency constraints apply?
- Quality and test strategy
  - What tests validate each dimension?
  - What must be testable independently and what needs full-stack coverage?
- Delivery and migration
  - What rollout and rollback strategy is required?
  - What migration/compatibility constraints exist?

## Synthesis rules

- Convert each answered dimension into concrete plan tasks.
- Mark each task with owner, inputs, expected outputs, and exit checks.
- Make assumptions explicit and assign owners for validation.
- Prioritize by impact on correctness, user-facing behavior, and unblock risk.

## Stop criteria

- At least one task exists for each non-optional dimension.
- No unresolved “must-have” decision remains unassigned.
- The plan includes validation for each major dimension.
- Cross-dimension conflicts are resolved before implementation.
