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
3. Use the check scopes in [feature-test](../feature-test/SKILL.md). Add missing focused regression coverage and run focused checks as needed while editing.
4. Inspect the diff and commit only the ticket's changes. Collect handoff evidence on the clean committed head. Use the child-task check scope for delegated work and all applicable required checks for standalone work. Preserve existing unrelated failures in the report. Report the exact base, commit range, and tested head SHA.
5. Return the handoff below. Do not open per-child PRs or deliver directly to the target. Push a worker branch only if the worker orchestrator explicitly assigns an authorized branch transfer. The worker orchestrator owns integration and reviewer dispatch; do not merge into the default branch or approve your own code.
6. For a repair, read the findings batch, reproduce relevant failures, and fix their shared causes across affected uses within scope. Preserve finding IDs in the response. If integration has advanced, use the assigned updated base and explain any conflict resolution. On the new committed head, rerun affected checks and required handoff checks. Return the head and evidence to the coordinator for integration and any required review follow-up.

```text
TICKET: T-001
RESULT: Ready for integration | Blocked
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
