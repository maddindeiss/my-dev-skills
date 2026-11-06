---
name: feature-test
description: Verify a feature ticket or integrated feature against its acceptance criteria and repository checks. Use before independent review and after changes made to address review findings.
---

# Feature verification

Adapted from Blueprint's test skill; see LICENSE.

Read the applicable Requested skills entries and effective skills revision from the assignment. Load relevant skill guidance for verification, carry its checks into the evidence, and report missing required capabilities. Read the assigned ticket or whole-feature spec and repository check instructions. Record the tested SHA and any dirty changes; a clean committed head is required for a final handoff.

1. Map each in-scope acceptance criterion to a real check through its public entry point. Cover relevant denied access, invalid input, failures, retries, and concurrency. Do not invent successful evidence for unavailable services.
2. Run the repository's required format, lint, type, build, and test checks as applicable. Start focused while fixing; run required wider checks before handoff. Preserve useful failure output.
3. When implementation is in scope, add missing regression coverage. For a significant new regression test, verify it fails against the old behavior when feasible. Perform any mutation or revert only in a disposable copy; never revert the implementation or integration worktree. A build failure from missing new interfaces alone does not prove the behavioral assertion catches the bug.
4. For browser-visible changes, exercise relevant flows in a real browser, including failure states, mobile and desktop widths, keyboard use, and console errors. Save evidence in the project's normal artifact location. If browser tooling is unavailable, mark those criteria Unverified.
5. Fix failures introduced by the change when acting as an implementer. In a review context, report failures without repairing production code. Respect the worker orchestrator's repair budget and never weaken tests to obtain a pass.

Return the skills revision and per-skill verification evidence alongside a criterion matrix (`Pass`, `Fail`, or `Unverified`), tested SHA, commands/results, artifacts, and pre-existing failures. Testing evidence is not independent review approval. Whole-feature verification must exercise interactions across tickets, not just concatenate their individual results.
