---
name: "make-a-plan"
description: "Create a small, outcome-first implementation plan that expands for real gaps, then contracts to the simplest direct path. Use for implementation planning, not audits or exhaustive inventories unless requested."
---

# Make A Plan

Create an active `development-patterns:plan-checklists` plan that gives an implementer the shortest safe path to the requested outcome.

For PersonaStack, create the active checklist in the workspace root at `.plans/in-progress/<repo>/YYYY-MM-DD/<PLANNAME>.md`. Use the GitHub repository name, or `agent-root` for workspace-wide work. Follow `development-patterns:plan-checklists`, root `AGENTS.md`, and `.plans/README.md` for lifecycle and archival. Completed plans belong at `.plans/completed/<repo>/YYYY-MM-DD/<PLANNAME>.md`.

Load applicable project guidance and only the authority needed for the change. Use `logic-patterns:1-3-1` and `development-patterns:centralized-fix-selection` only when a real design choice exists. Do not manufacture alternatives for a straightforward extension of an existing path.

## Frame the outcome

Before drafting, state:

- The named user, administrator, or service journey that must work.
- The observable success condition and the smallest test evidence that proves it.
- The existing canonical codepath to extend.
- Explicit constraints and non-goals.

Ask only about an unknown that materially changes the outcome. Otherwise record a reasonable assumption.

Do not make an existing route, store, process, compatibility mode, audit, migration inventory, cross-service consumer, or abstraction a requirement without evidence that the journey needs it.

## Expand once, then classify

Make one bounded expansion pass for missing dependencies that can block the named journey. Classify each item as one of:

- `now`: necessary for this journey.
- `later phase`: required outcome work owned by a later named journey.
- `defer`: useful but not required now. Record it as a concise defer item or tracker issue.
- `block`: needs a user decision or an external change.

Do not repeat expansion until no imaginable detail remains. Do not create exhaustive route, schema, keyspace, documentation, or test inventories unless the user explicitly requested an audit or those artifacts are necessary to enforce a stated safety boundary.

## Contract to the direct design

Run one contraction pass before finalizing the plan.

- Prefer the existing path plus a small logic gate over a new codepath.
- Use one authority, one mutation path, and one source of truth when the variants share ownership and lifecycle.
- Add a package, table, protocol, durable state machine, compatibility layer, or cross-service contract only when the named journey cannot work without it. State why the existing path cannot represent it.
- Remove a proposed abstraction when it adds more concepts than it removes for the current consumers.
- Keep personal, organization, and other minor variants in the same path when their difference is a fact, not a different lifecycle.
- Record a small complexity budget: changed owners, new persistent state, new packages, and old paths deleted. Reconsider the design when the implementation needs substantially more than this budget.

For a material architecture choice, compare the few realistic options for correctness, simplicity, integration, failure risk, rollback, and code removed or added. Select one canonical path and record the rejected complexity, not a long decision history.

## Plan the work

Use phases only to deliver complete, named journeys or coherent resource families. A phase does not need to be independently shippable while outward dependencies are being built. It must not leave a temporary compatibility wrapper as its durable outcome.

When a feature changes a shared contract, change that core contract first.
Use the strongest mechanical check available to find every dependent caller.
Repair those callers before building outward. Do not keep an optional old path
solely to let incomplete callers continue working.

Make each phase one implementation and review unit. It delivers one customer or
administrator feature, or one coherent resource family. Do not combine several
unrelated features merely because they share a schema or authorization layer.
When a plan requests independent review, define phases so the reviewer can
judge that complete feature without being asked to critique unfinished later
work.

For each phase, specify:

- Journey and acceptance condition.
- Existing entrypoints and ownership to change.
- The canonical implementation path and any justified logic gate.
- Any shared-contract closure order and mechanical check.
- Explicit non-goals and later-phase dependencies.
- Focused validation: the lowest-cost unit, store/command, registered-handler, client, or UI test that proves the behavior. Add stateful in-process workflows when behavior spans requests or mutations.
- Only applicable failure, authorization, migration, rollback, observability, and documentation work.

Keep API, browser, and service consumers in the same phase only when the journey needs all of them. Do not turn every future resource migration into a prerequisite for the first vertical slice.

## Final plan format

Persist only the converged plan:

- Objective and required outcomes.
- Selected direct design and rationale.
- Non-goals and defer list.
- Ordered journey/resource-family phases.
- Focused validation and acceptance evidence.
- Material blockers or risks.

Include deployment, release, or production migration only when the user asked for it or an applicable authority requires it.
