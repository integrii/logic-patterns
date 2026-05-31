---
name: "session-review"
description: "Use when you need to convert session history into reusable, repo-specific workflow skills from repeated service-specific discovery."
---

# Session Review

## Trigger
- explicit `$logic-patterns:session-review`
- when handoff, context reset, or major feature completion happens
- when repeated service-specific discovery appears likely to recur

## Macro Pattern: Session to Skill
- Capture recurring discovery with evidence from the session.
- Distill it into a small reusable rule.
- Save it as:
  - a checklist item inside an existing skill if one already owns the area
  - or a new skill only when scope is distinct

## Macro Pattern: Candidate Discovery Capture
- For each candidate:
  - record the command/lookup path
  - record why it was needed
  - record the decision outcome
  - record what made it reusable

## Macro Pattern: Skill Consolidation
- Before adding a new skill, search for overlap with existing skills.
- If overlap is substantial, add only the missing checklist/macro to the nearest existing skill.
- If unique, add one narrowly-scoped skill with the same structure (macro + checklists only).

## Session Review Checklist
- [ ] Identify at least 3 durable patterns from the session.
- [ ] Mark each as repeatable, repo-specific, and low-overlap.
- [ ] Group overlapping findings; eliminate duplicates.
- [ ] Update existing skills where domain already exists.
- [ ] Create a new skill only if no existing container matches.
- [ ] Ensure new/updated skill uses only macro patterns and checklists.
- [ ] Add a one-line README entry for discoverability.
- [ ] Verify examples and data-specific steps were not codified as hardcoded logic.

## New Skill Authoring Checklist
- [ ] Use one narrow intent and one trigger case.
- [ ] Add minimal frontmatter (`name`, `description`) plus macro/checklist sections.
- [ ] Use concrete decision rules, not prose-heavy narratives.
- [ ] Keep patterns reusable across multiple future sessions in the same repo.
- [ ] Validate no behavioral instructions are hidden in the skill.

## Maintenance Checklist
- [ ] Review `session-review` output monthly or when repeated confusion appears.
- [ ] Merge overlapping skills instead of adding another adjacent one.
- [ ] Remove stale, unreused findings after two release cycles without hits.
