# One aggregate delivery

Assemble child-task work on one request integration branch. Internal merges/cherry-picks are staging operations. Deliver one aggregate branch/PR to the named target; never merge child branches into that target individually.

## Prepare

1. Require tests and independent combined review for the entire selected scope. Record reviewed source SHA, target ref and observed SHA, intended method, and authorization in run.md. Use the repository's permitted method: one squash commit when requested or conventional, otherwise one aggregate merge/fast-forward operation.
2. Refresh the target. If it advanced, build a candidate including the updated target in a disposable worktree. Reconcile the spec and plan's [implementation status](protocol.md#implementation-status) with the resulting scope. Leave the target untouched. Assign conflicts to one designated worker, or resolve purely mechanical conflicts yourself in the candidate. Record resolutions; do not discard either side without understanding the contract.
3. A changed base or conflict resolution requires tests of the resulting combined tree and fresh independent review against the current target. Give the reviewer the candidate's check evidence. Route findings to workers and repeat within the final repair budget. Earlier approval cannot approve new resolutions.
4. If commits are rewritten for squash/rebase, verify the resulting tree matches the tested candidate and record the new source SHA. Obtain fresh review if content changes. Preserve previous branches as evidence.

## Update the target once

- Before the target update, ensure every direct-user skill update is acknowledged by main and final review covers the current requirements. Return a compact Ready to merge checkpoint with request revision, canonical skills revision/full requirements, approved source SHA, and target, then wait for the main orchestrator's acknowledgment of that current scope. Main can acknowledge automatically within existing user authorization. A newer request or skills revision, or a pending skill update, pauses affected delivery; changed requirements return to the separate spec/plan phase, while scheduling changes update assignments and affected verification; a changed candidate requires a new acknowledgment. This check does not grant merge authority or replace Git/CI gates.
- Prepare-only mode returns Ready to merge. PR mode publishes one aggregate PR when authorized and leaves it open unless merging is authorized too.
- An authorized remote merge requires current CI, required human approvals, resolved actionable reviews, and normal mergeability. Bind the merge operation to the expected PR head; never bypass protection. Rebuild and reverify if target movement invalidates proof.
- For an authorized local merge, use the target's clean worktree and verify HEAD still matches the recorded target SHA. Perform the repository-approved merge/fast-forward of the approved candidate. Do not reset a dirty checkout or move a branch behind another worktree. Serialize target writers; if exclusive access or an atomic expected-ref update cannot be ensured, report the blocker rather than racing another merge. A target mismatch returns to preparation.
- Record merge intent (source SHA, target-before SHA, PR, method) before the operation. Verify the actual target/merge SHA afterward. After interruption, inspect PR status or Git history/tree against that intent before retrying. Squash does not preserve source ancestry: compare the recorded operation and patch/tree too. If uncertain, stop instead of merging twice.
- Mark Complete only after the requested delivery state is verified. Report the single PR/merge result, proof and artifact locations to the main orchestrator. A blocked external gate returns Ready to merge or Blocked, with the exact remaining action.

Keep final review reports and run evidence local as described in [the delivery protocol](protocol.md). Merging does not authorize deployment or release publication.
