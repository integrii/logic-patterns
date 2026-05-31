---
name: "plan-checklists"
description: "Use when creating, maintaining, or closing markdown checklist plans for implementation work. Follow repo AGENTS storage policy: by default, place active plans under `.plans/$REPONAME/in-progress/$PLANNAME` and completed plans under `.plans/$REPONAME/completed/YYYY-MM-DD/$PLANNAME`."
---

# Plan Checklists

Use this skill when work should be tracked in markdown checklist plans.

This skill reflects the common practice across the child service repositories:
- track implementation work as markdown checklist plans in the location required by repo `AGENTS.md`
- keep active plans in `.plans/$REPONAME/in-progress/$PLANNAME` by default
- move completed, abandoned, or superseded plans to `.plans/$REPONAME/completed/YYYY-MM-DD/$PLANNAME`
- read `ADR.md` and `SPEC.md` before planning architecture or object-model changes when the repo requires that

## Core Rules

- Put active implementation plans in `.plans/$REPONAME/in-progress/$PLANNAME` unless repo policy explicitly requires a different local work queue.
- `$REPONAME` is the GitHub-style `org/repo` string for the owning repository.
- Use markdown checklists, not prose-only notes.
- Keep plans concise and task-oriented.
- One active file should represent one active stream of work.
- Update the checklist as work progresses; do not leave it stale.
- If a plan is executed through `$development-patterns:implement-plan`, track a single linked issue only in that workflow and do not create a second issue here.
- When starting a plan through that workflow, reference the associated tracker issue in the plan file.
- Move finished, abandoned, or superseded plans to `.plans/$REPONAME/completed/YYYY-MM-DD/$PLANNAME` promptly.
- Do not leave inactive plans sitting in the active `in-progress` queue.
- Do create repo-local `.plans/` as the default plan location unless repo policy explicitly requires a different location.
- Do not include deployment or shipping procedures in plans (for example, full build steps, container image publishing, Kargo/Argo workflow, cluster rollout, or live deploy actions).
- Include testing tasks only as targeted verification activities.

  - Targeted Playwright tests are encouraged.
  - Targeted `go test` (or equivalent) commands are encouraged.
  - Do not list full build/e2e container pipelines in routine implementation plans; those are reserved for the dedicated shipping agent.

## Before Writing a Plan

1. read the repo `AGENTS.md` (especially plan-storage section)
2. read `ADR.md` if the repo requires it before planning
3. read `SPEC.md` before architecture or object-model planning
4. confirm whether the work is repo-local or cross-repo

## Plan Content

A good checklist plan should usually include:
- a short title
- a brief scope line if needed
- concrete checklist items for implementation slices
- verification items when verification is part of the work
- follow-up or cleanup items only if they are real required work

Prefer:
- small, concrete, verifiable checklist items
- wording that maps to actual code or docs changes
- slices that can be completed and committed independently

Avoid:
- long narrative design essays
- duplicated background from `ADR.md`, `SPEC.md`, or `ARCHITECTURE.md`
- vague items like `fix stuff` or `handle edge cases`
- mixing unrelated workstreams into one checklist

## Lifecycle

- create the plan in the path mandated by repo policy when work starts (normally `.plans/$REPONAME/in-progress/...`)
- revise the checklist as scope changes
- mark items complete as the work lands
- move the file to the matching completed bucket when the plan is done, abandoned, or replaced
- when completed by `$development-patterns:implement-plan`, archive only after every checklist item is addressed and the plan-linked tracker issue is moved to the appropriate review/completion state unless the user explicitly approved completion.

## Cross-Repo Note

If the workspace root has its own plan-storage policy, follow that policy for cross-repository work and repository-local work in that workspace.

## Output Expectations

When you create or update a plan, report:
- where the plan file lives
- whether it is repo-local or cross-repo
- whether the plan has a single plan-linked tracker issue and current issue state if managed by `$development-patterns:implement-plan`
- any required `ADR.md` / `SPEC.md` alignment that shaped the checklist
