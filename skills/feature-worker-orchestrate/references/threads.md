# Agent operations and handoffs

Discover available tools by capability; names and schemas may change. Do not invent tools or assume a command in a source repository exists here.

## Visible Codex chats

- Read current `create_thread`, `list_projects`, `list_threads`, `read_thread`, `wait_threads`, and `send_message_to_thread` schemas. Use the project ID returned by discovery. Follow tool-specific requirements for human authorization, worktree creation, starting refs, and messaging.
- Prefer project worktree chats when the user explicitly requested separate worktrees and the project is a Git repository. Do not pass an invented branch through a field that requires a user-specified branch. If the tool creates from the default branch, tell the worker to inspect the clean managed worktree and create its ticket branch at the supplied, verified integration SHA before implementation. Do not reset unrelated work to make it fit.
- For a pending creation, record the client ID, then use supported discovery/wait mechanisms to obtain the real thread ID. Do not dispatch a duplicate while setup is pending.
- Use `wait_threads` with thread ID, host ID, and latest cursor; use bounded waits up to 60 seconds. Use `read_thread` for missing details or intervention. A timeout is not a failure or a completed result.
- The worker orchestrator collects workers' final messages through snapshots. Workers and reviewers need not message it back. This avoids assuming a worker orchestrator's prompt grants reverse messaging authority.
- Use authorized `send_message_to_thread` to send findings to the existing implementation chat. Preserve model settings unless the user requests an override. If that chat is unavailable, first verify its last worktree/head and record a replacement before starting another.
- For review, create a fresh chat in its own worktree at the reviewed SHA, or use a fresh subagent if the user authorized that review mode. Never ask the implementer to approve its own work.

Direct user skill instructions received by the worker orchestrator are reported to main immediately using [requested skills](requested-skills.md). Send a compact event directly to main only with actual user authorization for that direction; otherwise publish it in a checkpoint for collection and keep it pending until acknowledged. Read the main chat ID/host from run.md, never infer a destination.

## Internal subagents

Use only when the user accepts this mode and delegation is available. `spawn_agent` creates workers/reviewers; result messages return to the worker orchestrator, and `followup_task` resumes a worker for fixes. Isolate each worker's Git worktree before dispatch; do not rely on separate context to isolate files. Give the exact working directory and restrict edits to it. Respect the host's active-agent limit, including the main orchestrator, worker orchestrator, workers, and reviewer. Read-only reviews still need their own temporary checkout if running tests can modify tracked files.

## Implementation prompt

Fill all fields with concrete values; pass the skill's absolute path as well as its name.

```text
Use $feature-implement at [absolute SKILL.md path].
Implement ticket [ID] for feature [name] in [exclusive worktree].
Start from integration commit [full SHA]; use ticket branch [branch].
Spec: [path and current content or immutable revision].
Ticket: [path and full current content].
Repository instructions: [locations]. Checks: [known commands].
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
Base: [full SHA]. Head: [full SHA].
Spec and ticket: [current content and revision].
Check commands: [commands]. Repository instructions: [locations].
Scope: [ticket acceptance or entire feature acceptance matrix].
Requested skills (optional): [applicable entries/sources/scopes and implementation evidence].
Skills revision: [effective revision being reviewed].
Read the skill standards and verify compliance while staying read-only.
Previous findings if this is a repair review: [IDs and reports].
Return findings and a verdict in this chat; the worker orchestrator collects it.
Do not repair code or modify the implementer's worktree.
```

## Repair prompt

Send to the existing worker with exact finding IDs, evidence, reviewer report, current integration SHA, and the ticket's remaining repair budget. Ask it to fix only the in-scope issues, rerun affected checks, commit, and return the new head. Include resolved decisions and the full applicable Requested skills entries/current revision, including changes since the previous assignment. Do not tell it to satisfy a reviewer by weakening tests or changing accepted behavior. For a dispute, request a reproduction or counter-evidence for the worker orchestrator to evaluate.
