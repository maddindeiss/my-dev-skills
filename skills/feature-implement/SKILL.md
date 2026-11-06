---
name: feature-implement
description: Implement one assigned child task or feature ticket in an assigned isolated worktree and return committed code with verification evidence. Use for a worker-orchestrator assignment or a targeted ticket repair.
---

# Feature implementation worker

Adapted from Blueprint's task-to-pr workflow; see LICENSE.

Read [the delivery protocol](../feature-worker-orchestrate/references/protocol.md), repository instructions, current ticket/spec, dependency evidence, and code touched by the task. The assignment's current ticket content takes precedence over an older checkout copy; conflicting sources go back to the worker orchestrator.

Read [requested-skill propagation](../feature-worker-orchestrate/references/requested-skills.md). Before implementation, load each applicable requested skill from its supplied source and apply its instructions within the assigned scope. Report missing skills or conflicting instructions to the worker orchestrator instead of silently skipping them. On a skill update, recheck existing output and report compliance under the new effective revision.

1. Verify the assigned worktree, branch, and base SHA before edits. Never work in the worker orchestrator's integration worktree. If the requested base is unavailable or the checkout contains unexplained changes, report the blocker instead of resetting it.
2. Implement only the ticket's outcome. Follow existing contracts and update affected documentation. Ask the worker orchestrator about product decisions or scope conflicts; routine implementation choices are yours.
3. Add focused tests for changed behavior, regressions, and relevant failure paths. Use [feature-test](../feature-test/SKILL.md) until the required evidence is available. Preserve existing unrelated failures in the report.
4. Inspect the diff and commit the ticket's changes. Report the exact base, commit range, and head SHA. Do not include unrelated working-tree changes or shared coordination records.
5. Return the handoff below. Do not open per-child PRs or deliver directly to the target. Push a worker branch only if the worker orchestrator explicitly assigns an authorized branch transfer. The worker orchestrator owns integration and reviewer dispatch; do not merge into the default branch or approve your own code.
6. For a repair, read every finding, reproduce relevant failures, and fix their cause. Preserve finding IDs in the response. If integration has advanced, use the assigned updated base and explain any conflict resolution; all changed code needs fresh review. Return to testing before handing back.

```text
TICKET: T-001
RESULT: Ready for review | Blocked
WORKTREE / BRANCH:
BASE_SHA / HEAD_SHA:
COMMITS:
SKILLS_REVISION:
REQUESTED_SKILLS: ID/source → applied actions and evidence, or Not applicable with reason
ACCEPTANCE: criterion → evidence
CHECKS: command → result
FINDINGS_ADDRESSED: ID → fix or counter-evidence
UNVERIFIED:
BLOCKER:
```

Write the result in your current chat for collection. Keep your branch and worktree available. Do not update shared ticket statuses or retry beyond the worker orchestrator's recorded budget. If the worker cannot finish during a run, report the exact partial state and next step rather than claiming completion.
