# Agent operations and handoffs

Discover available tools by capability; names and schemas may change. Do not invent tools or assume a command in a source repository exists here.

## Chat titles

Start every visible delivery chat title with its role. Use a short, plain-language scope label across the request, such as `Saved searches`. When the request covers part of a feature, describe the selected outcome. Label each child task by what it changes, such as `Add save button`, and use distinct descriptions for different tasks. Keep ticket and assignment IDs in the coordination records and handoffs.

- `Main Orchestrator — <scope>`
- `Worker Orchestrator — <scope>`
- `Implementation Worker — <scope> / <task description>`
- `Reviewer — <scope>`

Keep the reviewer title stable across repair follow-ups. Record review scope and round in the reports. An explicitly required separate early review may use `Ticket Reviewer — <scope> / <task description>`.

Pass the title in `create_thread.title` so it appears from creation, including while a worktree is pending. If creation has no title field, use `set_thread_title` once the real thread ID resolves. On resume, reuse the recorded descriptive scope label. If the label is missing or uses ticket IDs, derive and record a descriptive label from the existing scope. Apply the convention to reused chats with default titles or earlier skill-generated titles that use IDs. Preserve titles explicitly chosen by the user. For a replacement with the same base title, append ` / Attempt 2` or the next attempt number.

## Visible Codex chats

- Read current `create_thread`, `set_thread_title`, `list_projects`, `list_threads`, `read_thread`, `wait_threads`, and `send_message_to_thread` schemas. Use the project ID returned by discovery. Follow tool-specific requirements for human authorization, worktree creation, starting refs, and messaging.
- Prefer project worktree chats when the user explicitly requested separate worktrees and the project is a Git repository. Do not pass an invented branch through a field that requires a user-specified branch. If the tool creates from the default branch, tell the worker to inspect the clean managed worktree and create its ticket branch at the supplied, verified integration SHA before implementation. Do not reset unrelated work to make it fit.
- For a pending creation, record the client ID, then use supported discovery/wait mechanisms to obtain the real thread ID. Do not dispatch a duplicate while setup is pending.
- Use `wait_threads` with thread ID, host ID, and latest cursor; use bounded waits up to 60 seconds. Use `read_thread` for missing details or intervention. A timeout is not a failure or a completed result.
- The worker orchestrator collects workers' final messages through snapshots. Workers and reviewers need not message it back. This avoids assuming a worker orchestrator's prompt grants reverse messaging authority.
- Use authorized `send_message_to_thread` to send findings to the existing implementation chat. Preserve model settings unless the user requests an override. If that chat is unavailable, first verify its last worktree/head and record a replacement before starting another.
- Create one independent reviewer chat when review is due and reuse it for follow-ups and combined review. Preserve evidence before moving its isolated checkout to the new reviewed SHA. Replace a reviewer only when unavailable or no longer independent, after reconciling its pending work and preserving its report. An approval request belongs to the user and must be resolved through its normal approval path. Use internal review only when explicitly selected by the user.

Direct user skill instructions received by the worker orchestrator are reported to main immediately using [requested skills](requested-skills.md). Send a compact event directly to main only with actual user authorization for that direction; otherwise publish it in a checkpoint for collection and keep it pending until acknowledged. Read the main chat ID/host from run.md, never infer a destination.

## Internal subagents

Use only for implementation or review roles the user explicitly assigned to internal subagents for this request. `spawn_agent` creates workers and reviewers, result messages return to the worker orchestrator, and `followup_task` resumes a worker for fixes. Isolate each worker's Git worktree before dispatch. Give the exact working directory and restrict edits to it. Respect the host's active-agent limit, including the main orchestrator, worker orchestrator, workers, and reviewer. Read-only reviews still need their own temporary checkout if running tests can modify tracked files.

## Implementation prompt

Fill all fields with concrete values; pass the skill's absolute path as well as its name.

```text
Use $feature-implement at [absolute SKILL.md path].
Implement ticket [ID] for feature [name] in [exclusive worktree].
Start from integration commit [full SHA]; use ticket branch [branch].
Spec: [path and current content or immutable revision].
Ticket: [path and full current content].
Repository instructions: [locations]. Ticket checks: [focused commands and checks required before handoff].
Dependencies already integrated: [IDs and SHAs].
Scope and ownership: [outcome, permitted changes, shared contracts].
Requested skills (optional): [applicable IDs, names, resolved sources, roles, scopes; or None].
Skills revision: [effective revision, including pending event IDs].
Read and apply those skills; report per-skill evidence or a missing-skill blocker.
Delivery mode: return committed child work for aggregate integration; no child PR.
Return the result in this chat. The worker orchestrator will collect it.
Do not edit ticket/run/review records, integrate other tickets, or merge
into the default branch. Stop and report any unresolved product decision.
```

## Review prompt

```text
Use $feature-review at [absolute SKILL.md path].
Independently review [ticket ID or entire feature] in [review worktree].
Review scope: [combined delivery or the specific required early gate and its reason].
Base: [full SHA]. Head: [full SHA].
Spec and ticket: [current content and revision].
Verification evidence: [commands, results, tested SHA, and artifact locations].
Checks required independently: [commands, if any]. Repository instructions: [locations].
Scope: [ticket acceptance or entire feature acceptance matrix].
Requested skills (optional): [applicable entries/sources/scopes and implementation evidence].
Skills revision: [effective revision being reviewed].
Read the skill standards and verify compliance while staying read-only.
Previous findings if this is a repair review: [IDs and reports].
For a follow-up: [previous reviewed SHA, repair diff, and changed requirements].
Return findings and a verdict in this chat; the worker orchestrator collects it.
Do not repair code or modify the implementer's worktree.
```

## Repair prompt

Send to the existing worker with exact finding IDs, evidence, reviewer report, current integration SHA, and the ticket's remaining repair budget. Ask it to fix only the in-scope issues, rerun affected checks, commit, and return the new head. Include resolved decisions and the full applicable Requested skills entries/current revision, including changes since the previous assignment. Do not tell it to satisfy a reviewer by weakening tests or changing accepted behavior. For a dispute, request a reproduction or counter-evidence for the worker orchestrator to evaluate.
