# CLAUDE.md — {{Project Name}} Agent Contract

> The top-priority document Claude Code auto-reads every session. The rules here take precedence over all other instructions. Detailed procedures are in `HARNESS.md`; the invariants themselves are in `INVARIANTS.md`.

## Project one-liner
{{What does this project build — one sentence}}

## Behavior every session
1. First read `PROGRESS.md` to grasp the current phase, next tasks, and open questions.
2. **One phase at a time.** Start only when the entry condition (DoR) in that `phases/Mn.md` is met.
3. **Done = all measurable gates (DoD) pass.** "Runs without errors" is not done.
4. When claiming a gate passed, record the **command and output as evidence** in the phase file / `PROGRESS.md`.
5. At the end, update `PROGRESS.md` and commit.

## Red lines (never — stop immediately on violation)
- Faking a "pass" by deleting/weakening tests or arbitrarily lowering a gate's numbers.
- Proceeding to the next phase with parity or an invariant (`INVARIANTS.md`) broken.
- Arbitrarily changing architecture or package boundaries (without user approval). Boundaries are in `docs/FILE_TREE`.
- Committing secrets (`.env`) or large artifacts (weights, binaries).
- Weakening guardrails (policy violations, excessive requests, excessive permissions).

## STOP — stop work and report if any of these
- The same failure remains unsolved after 3 attempts via different approaches.
- You can't clear a DoD gate and feel the urge to work around it.
- Passing would require breaking an invariant.
- A large architectural change appears necessary / an external constraint (resources, access, terms).
-> The procedure for stopping is in `HARNESS.md` §2.

## Where things live
The file map and session protocol are in `README.md`. The verification priority is in `HARNESS.md` §3.
