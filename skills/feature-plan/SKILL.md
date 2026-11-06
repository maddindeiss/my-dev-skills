---
name: feature-plan
description: Prepare an implementation plan and dependency-ordered Markdown tickets from an existing specification before orchestration begins.
---

# Feature plan and tickets

Adapted from Blueprint's plan skill; see LICENSE.

Read the spec, repository instructions, and affected code. Read [the delivery protocol](../feature-worker-orchestrate/references/protocol.md) for shared paths and statuses. If a missing decision blocks a ticket, use [feature-spec](../feature-spec/SKILL.md) for that decision; continue independent planning.

Keep tickets in `docs/features/<feature-slug>/tickets/` and write `plan.md` beside `spec.md`. Follow an existing equivalent layout if supplied. Reuse existing tickets by ID and outcome; do not duplicate them on reruns. Do not create external issues unless requested.

Prepare the canonical plan before delivery starts. Preserve the selected parent scope and IDs when planning a ticket or function. Include planned subtasks where useful. During later execution, the worker orchestrator may divide this decided work into assignments, but it does not invoke this skill or rewrite this plan. Changes to scope or design return here as a separate preparation step.

Preserve optional skill selections from the spec or user request in the plan and relevant tickets, with scope and source where known. Do not invent a skill requirement for a task that has none; an omitted field does not block planning or delivery.

## Split the work

- Each ticket delivers one end-to-end result with its proof. Prefer a small usable first slice that establishes the important contracts.
- Map every feature acceptance criterion to at least one ticket. Shared criteria may span tickets, but name who verifies the complete flow.
- Assign stable `T-n` IDs for feature-level tickets and parent-scoped `W-n` IDs for worker tasks. Never renumber existing tickets or confuse a child task with its parent.
- Record dependencies by ID. Check for missing IDs, self-dependencies, and cycles before declaring the plan ready.
- Mark tickets with unresolved blocking decisions `Blocked`, with the missing decision named. Only actionable tickets become `Ready`.
- Inspect likely files, contracts, schema changes, generated files, and lockfiles. Mark overlapping work as sequential unless the ownership boundary makes concurrent editing safe. Put shared contract changes before their consumers.
- Keep a ticket small enough for one worker and one reviewable commit series. Separate refactoring only when it would obscure the behavior change.
- Reserve a final whole-feature verification and review after integration; passing ticket reviews alone do not complete a feature.

## Plan format

Record the spec path, intended base branch, proposed feature integration branch, check commands discovered in the repository, and this table:

```markdown
| Ticket | Outcome | Depends on | Acceptance | Likely files/contracts | Parallel group |
| --- | --- | --- | --- | --- | --- |
```

Explain the dependency order, any intentionally sequential work, and unresolved decisions. Identify real commands; for tests that do not yet exist, describe the entry point and expected result instead of pretending they ran.

## Ticket format

```markdown
# T-001: Action and user-visible result
Status: Ready
Spec: ../spec.md
Parent: source ticket/function path or None
Requested skills (optional): inherited/user-selected skill IDs and scope, or omit
Depends on: None
Acceptance: AC-1

## Outcome and scope
What works after this ticket, why it matters, and what is excluded.

## Implementation notes
Relevant files, contracts, existing patterns, and failure rules.

## Acceptance and proof
Observable criteria, affected failure paths, and commands or scenarios.

## Handoff
Worker, worktree, branch, base SHA, head SHA, review path, integration SHA,
and blocker. Initially unassigned; the worker orchestrator maintains this section.
```

Return the existing spec path, completed plan and ticket paths, parallel groups, and blockers. Stop before orchestration or implementation. Once both documents are ready, the user starts [feature-orchestrate](../feature-orchestrate/SKILL.md) in the delivery phase. For a revision requested after delivery was blocked, identify changed requirements, dependencies, and document revisions so the orchestrators can invalidate affected evidence.
