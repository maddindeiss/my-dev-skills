---
name: feature-spec
description: Define a feature before implementation with user stories, technical decisions, and testable acceptance criteria. Use to turn an idea into a specification for the feature delivery skills.
---

# Feature specification

Adapted from Blueprint's requirements and spec skills; see LICENSE.

Write `docs/features/<feature-slug>/spec.md`, or update the user's existing specification in place. Preserve repository conventions and existing requirement IDs. The specification is the product and technical source of truth for this feature.

1. Read the request, repository instructions, relevant code and tests, and existing requirements and architecture. Verify claims about current behavior against code.
2. Resolve choices that materially change users' behavior, data, interfaces, security, or delivery scope. Ask only for decisions that cannot be inferred; offer a recommendation. Record reasonable implementation assumptions without interrupting work.
3. Describe success, invalid input, denied access, failure, and retries where applicable. Choose the smallest design that meets the need. Explain consequential alternatives and tradeoffs.
4. Fill the template below, omitting sections that add nothing. Preserve `AC-n` IDs through revisions and never reuse retired IDs. Mark unresolved decisions and the criteria they block.
5. This is the preparation phase before orchestration. Stop after the specification, or continue to [feature-plan](../feature-plan/SKILL.md) if the user requested planning too. Return the completed document and unresolved decisions; do not start orchestrators or implementation workers. The user starts delivery after both spec and plan are ready. When revising documents for a blocked delivery, report the changed criteria and revision so affected work can resume explicitly.

```markdown
# Feature name
Status: Draft | Ready | Delivered

## Problem and users
Who needs this, what happens today, and the intended outcome.

## User stories
As a user, I want an action, so that I get an observable benefit.
Include affected existing requirement IDs when available.

## Technical design
Affected modules and interfaces, data ownership, contracts, migrations,
failure handling, and test entry points. Include a small Mermaid flow diagram
when it clarifies boundaries or ordering. Explain the main tradeoff.

## Acceptance
| ID | Observable outcome | How to verify |
| --- | --- | --- |
| AC-1 | A concrete result | Test or user flow and expected result |

## Requested skills (optional)
User-selected skill names, applicable scope and roles, and source when known.
Omit this section when no skills were selected; it does not block readiness.

## Out of scope
Explicit boundaries for implementation and review.

## Open decisions
Unanswered choice, recommendation, and what it blocks; or None.
```

Return the path, key decisions, and any blocking question. `Ready` means the definition is actionable, not that a human approved it. `Delivered` requires the final feature review and tests to pass; record whether delivery is local, in an open PR, or merged.
