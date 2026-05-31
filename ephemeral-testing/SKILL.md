---
name: "ephemeral-testing"
description: "Use when standing up a full local multi-service test stack for cross-service validation, with shared dependencies, deterministic namespaces, and controlled test lifecycle."
---

# Ephemeral Testing

Use this skill when you need a full local service graph available for end-to-end and integration testing.

## Core idea

The test stack is a reusable, deterministic containerized environment that represents the standard local test network for the product:

- one shared infra layer (datastores, caches, queues, shared services),
- one test target service image override per run,
- one reproducible network namespace and service discovery plan,
- one cleanup policy to avoid environment drift.

## Before starting

1. Confirm the service graph inventory and shared dependency contracts.
2. Check repository-local guidance (`AGENTS.md`, `SPEC.md`, `ADR.md`), then identify required harness variables.
3. Ensure test orchestration can consume:
   - a generated namespace ID,
   - generated stack metadata,
   - service image tags,
   - readiness checks and test selectors.

## Stack configuration

1. Define a stable namespace:
   - `STACK_ID` (required for stack identity).
   - Fall back to a stable runtime identifier only if explicitly allowed.
   - Fail fast if no namespace source is available.
2. Load shared stack defaults in one stack config source (for example, `.testing/.env`-style file).
3. Set testing mode explicitly:
   - enable ephemeral account APIs for e2e bootstrap (`TESTING_EPHEMERAL_API_ENABLED=true`);
   - set shared test secret(s) used by API bootstrap and browser/test clients consistently.
4. Keep service ports private by default.
   - only enable host binding when needed for local manual/browser debugging.

## Start flow

1. Resolve stack-specific values and expose them via a generated stack metadata file for dependent scripts.
2. Start all shared infra and application services from a compose-like manifest in a single namespace.
3. Block until explicit readiness checks pass for:
   - core infrastructure services (data stores, cache/broker),
   - all services required by the current test target.
4. Validate that discovery endpoints resolve by internal service DNS/hostnames used by all services.
5. Refresh dependency images from registry for non-local services by default.

## Image policy

1. Default to remote dependency images for deterministic shared-stack behavior.
2. Allow exactly one or more explicit image overrides for services under test.
3. Do not treat `latest`-style mutable tags as test gating truth in normal validation.

## Test execution

1. Run browser/API/synthetic suites against the same internal network and service routes used by services themselves.
2. Keep default e2e scope focused on:
   - first-party behavior that is stable locally,
   - seams with safe static/test doubles,
   - non-mutable flows unless a real provider sandbox contract exists.
3. Classify provider-heavy mutable flows as live-only unless they can be safely validated in this stack.

## Lifecycle and cleanup

1. Default behavior:
   - start → run → teardown.
2. Optional warm-stack path:
   - keep stack alive only for explicit debug sessions;
   - record the namespace and teardown manually with matching identifiers.
3. On stop/shutdown, remove ephemeral storage and mounted test state to prevent cross-run contamination.

## Troubleshooting checkpoints

- `STACK_ID` mismatch between test launcher and stack harness.
- Secret mismatch between test bootstrap and API routes (`ephemeral` operations fail).
- Missing readiness before test start.
- Service route mismatch between internal discovery and external/base URLs.

## Exit criteria

Proceed to test failure analysis only after:
- stack namespace is coherent,
- readiness gates are green,
- ephemeral test endpoints are available, and
- cleanup completes cleanly between runs.
