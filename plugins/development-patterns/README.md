<div align="center">
  <img src="logo.png" alt="development-patterns logo" width="380"/>
</div>

# development-patterns

Engineering-oriented workflows for implementation, architecture, testing, and execution quality.

## Skills in this plugin

### architecture-decision-records

Keep user-decided architecture choices in repository-local `ADR.md`. Load those decisions before design work and keep incidental implementation notes elsewhere.

### architecture-spec

Define cross-service ownership, authority, boundaries, and compatibility in the workspace architecture document. Keep concrete service behavior in each owning repository's `SPEC.md`.

### centralized-fix-selection

Find the narrowest shared control point for behavior that spans multiple callers. Prefer a centralized policy or shared fix when safe and preserve external behavior unless the requested outcome requires a change.

### ephemeral-testing

Run a reproducible multi-service test stack in one session-owned namespace. Require `STACK_ID` to equal `CODEX_THREAD_ID`, isolate outputs and resources, check readiness, and clean up between runs.

### github-release-builds

Build multi-architecture container images in GitHub Actions on semver tags and publish them to GHCR. Update example manifests, attach image release evidence, and document the tag-only release process in `RELEASE.md`.

### implement-plan

Execute an active plan through focused tests, a completion ledger, local review, and a clean adversary loop. Use GPT-6 Astra with medium reasoning for independent reviewers unless the user chooses otherwise. Close only after the final diff and required acceptance gates are verified.

### plan-checklists

Track implementation work with concise Markdown checklists and the repository's storage policy. Keep PersonaStack plans in the workspace root `.plans/` tree and archive inactive plans promptly. Keep shipping procedures outside implementation plans.

### spec-driven-development

Define desired behavior, contracts, and implementation constraints in repository-local `SPEC.md`. Keep service specs aligned with accepted architecture and use them to guide implementation and validation.
