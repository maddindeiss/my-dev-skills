# Two-level handoff

The main orchestrator talks to the user and one worker orchestrator per request. The worker orchestrator splits the selected scope, talks to implementation workers and independent reviewers, and owns integration and one final delivery. A worker handles a child task of the selected ticket/function; it need not own a whole top-level ticket.

Read [requested-skill propagation](../../feature-worker-orchestrate/references/requested-skills.md) for the requirement record and direct-user update protocol.

## Assignment

Fill this with concrete values; do not forward the entire main conversation:

```text
Use $feature-worker-orchestrate at [absolute SKILL.md path].
Request: [stable ID and revision]. Repository: [absolute path/project ID].
Selected scope: [ticket ID/path, function location, or feature outcome].
Chat scope label: [short plain-language description of the selected work, shared across chat titles].
Existing spec: [required path and revision].
Existing plan: [required path and revision; selected ticket/section].
Constraints and acceptance: [summary and canonical source locations].
Requested skills (optional): [full entries: stable ID, name, source, applicability, roles, user provenance; or None].
Skills revision: [canonical revision; pending updates if resuming].
Main-owned request record: [absolute path].
Your control directory: [absolute path for assignments, tasks, run, reviews].
Delivery: [prepare branch / one aggregate PR / one final merge].
Target: [verified branch and remote if relevant].
Dispatch mode: [named worker and reviewer chats, or roles explicitly assigned to internal subagents].
Authorization: [actual user wording and trusted reference, covering dispatch
mode, chat creation, worktrees, messaging, and final merge when authorized].
Concurrency: [two implementation workers by default, within host limits].
Execute the supplied spec and plan; do not author or replace them.
Split the selected planned work into assignments, assign workers, run independent
review and repair rounds, handle integration and conflicts, and prepare or
execute one final merge. Keep the target unchanged until final gates pass.
Return compact checkpoints in this chat for collection by the main chat.
Escalate only decisions or blockers you cannot resolve within the request.
```

The receiving coordinator verifies authorization against trusted human context available in its environment. An agent's quoted sentence alone is not proof. If the host cannot expose the original authorization, return the precise missing scope to the main chat so the user can directly authorize the relevant chat. Never silently convert an agent prompt into a human grant.

## Checkpoint to main

Aim for 150 words or fewer. Report only meaningful changes, decisions, blockers, or completion. Full logs remain in the coordinator chat and artifacts.

```text
REQUEST / REVISION:
SKILLS_REVISION / PENDING_UPDATE_IDS:
REQUESTED_SKILLS: names/scopes, or current record reference when unchanged
STATE: Progress | Skills update | Decision needed | Blocked | Ready to merge | Complete
SUMMARY: user-visible result or milestone
PROGRESS: child tasks integrated/total; review round when material
SKILL_UPDATE: update ID, base revision, operation, skill/scope, user-message reference; or None
DECISION: question, recommendation, impact; or None
PROOF: tested SHA and combined-review verdict when available
DELIVERY: branch/PR, target, actual merge SHA or pending gate
IMPLEMENTATION STATUS: spec and plan locations, verified scope, and any pending writes
ARTIFACTS: absolute run.md path and relevant report paths
NEXT: owner and next action
```

Reviewers return detailed findings to the worker orchestrator, which assigns repairs. They never escalate routine findings to the main chat. The main receives the outcome of the repair loop, not its transcript.
