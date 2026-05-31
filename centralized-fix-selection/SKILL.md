---
name: "centralized-fix-selection"
description: "Use when deciding whether a behavior change belongs in centralized policy, a shared abstraction, or a local call site. Prefer narrow high-leverage shared fixes for cross-cutting behavior."
---

# Centralized Fix Selection

When a bug or behavior spans multiple routes, handlers, pages, or integrations, look for the narrowest shared control point first.

Before deleting, renaming, or simplifying shared code, identify touched behavioral surfaces such as routes, DTOs, storage rows, events, commands, manifests, pages, packages, and tests. Do not use "forward-only" as permission to drop desired functionality. Preserve current external behavior unless the active request or authoritative docs require a desired-state change.

Evaluate the fix in this order:
1. centralized policy change
2. shared abstraction or helper change
3. local call-site patch

Prefer centralized fixes when the behavior is governed by shared concerns such as:
- auth
- session state
- routing
- request policy
- rendering bootstrap
- credential routing
- transport selection

Choose a local patch only when a centralized change would be unsafe, would violate an explicit spec, or the behavior is intentionally route-specific.

When proposing or implementing the fix:
- state which layer you chose
- state why the narrower shared layer is or is not safe
- prefer preserving existing call sites by changing boundary behavior when that is sound
