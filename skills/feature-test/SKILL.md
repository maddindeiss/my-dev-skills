---
name: feature-test
description: Verify a feature ticket, integration candidate, or integrated feature against its acceptance criteria and the checks required at that stage.
---

# Feature verification

Adapted from Blueprint's test skill; see LICENSE.

Read the applicable Requested skills entries and effective skills revision from the assignment. Load relevant skill guidance for verification, carry its checks into the evidence, and report missing required capabilities. Read the assigned ticket or whole-feature spec and repository check instructions. Record the tested SHA and any dirty changes; a clean committed head is required for a final handoff.

1. Map each in-scope acceptance criterion to a real check through its public entry point. Cover relevant denied access, invalid input, failures, retries, and concurrency. Do not invent successful evidence for unavailable services.
2. When implementation is in scope, add missing regression coverage. For a significant new regression test, verify it fails against the old behavior when feasible. Perform any mutation or revert only in a disposable copy. A build failure from missing new interfaces alone does not prove the behavioral assertion catches the bug.
3. Run the checks needed for the current stage. Follow any repository, user, or requested-skill requirements for that stage.

   - Child task — run focused checks for changed behavior and the checks required at ticket handoff. Use wider checks here when they are required at handoff or needed to verify the ticket.
   - Integration candidate — check affected interactions and shared contracts before advancing the branch or releasing dependent work.
   - Final integrated head or standalone verification — run the full set of applicable required format, lint, type, build, and test checks. For integrated work, exercise interactions across tickets.

   Reuse recorded passing results that cover the same command, exact head, relevant test environment and inputs, and current requirements. Rerun affected checks after changes and any checks the current gate requires on the new head. Record which results were reused and preserve useful failure output.

4. Verify browser-visible flows relevant to this stage with real-browser evidence. Cover failure states, mobile and desktop widths, keyboard use, and console errors where relevant to those flows. Save new evidence in the project's normal artifact location. If required browser evidence is unavailable, mark those criteria Unverified.
5. Fix failures introduced by the change when acting as an implementer. In a review context, report failures without repairing production code. Respect the worker orchestrator's repair budget and never weaken tests to obtain a pass.

Return the skills revision and per-skill verification evidence alongside a criterion matrix (`Pass`, `Fail`, or `Unverified`), tested SHA, commands/results, artifacts, and pre-existing failures. Independent review issues its own verdict.
