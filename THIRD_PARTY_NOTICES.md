# Sources and adaptations

## Blueprint

- Author: Owain Lewis
- Source: https://github.com/owainlewis/blueprint
- Revision: `fb5a850965039d434343fa1b74f8c7f47eb2bd96`
- License: MIT, Copyright (c) 2026 Owain Lewis
- Adapted files: `skills/requirements/SKILL.md`, `skills/spec/SKILL.md`, `skills/plan/SKILL.md`, `skills/task-to-pr/SKILL.md`, `skills/test/SKILL.md`, `skills/review/SKILL.md`, `skills/architecture-review/SKILL.md`, and `skills/codex-issue-coordinator/SKILL.md`.

The feature spec and ticket templates, vertical-slice planning, independent review, acceptance evidence, and bounded repair loops reuse and adapt this material. Changes include namespacing, Markdown tickets, local feature integration, explicit tool authorization, two-level orchestration with a compact main context, execution decomposition of preplanned work, one aggregate merge, durable orchestration state, disposable regression experiments, and review of the integrated feature. The HTML renderer and its dependencies are not included.

## Matt Pocock skills

- Author: Matt Pocock
- Source: https://github.com/mattpocock/skills/tree/main/skills/in-progress/software-factory
- Revision inspected: `4ae2317ddb22b046e26455e4f9c9b3aaaf62212d`
- License: MIT, Copyright (c) 2026 Matt Pocock
- Material consulted: `SKILL.md` and `RUNNERS.md` under `skills/in-progress/software-factory/`.

The orchestration design adapts the separation of implementation and review runs, durable external state, duplicate-work prevention, and capped retries. It uses local Codex chats and worktrees rather than introducing hosted runners, triggers, deployment, or observability services.

The MIT notices are preserved in the root LICENSE and in each distributable skill folder so copying individual folders retains the notices. Install the complete set for the cross-skill workflow.
