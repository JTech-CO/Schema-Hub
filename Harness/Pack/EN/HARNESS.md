# HARNESS.md — Master Operating Manual

> Not "what to build" (the whitepaper) but **"how to proceed, verify, and recover" (operational discipline)**. A phase is "done" not when the code runs but when its **measurable gates (DoD) pass**. If a gate doesn't block it, a wrong "done" verdict propagates into the next phase.

## 0. Session loop
1. Start: `PROGRESS.md` -> current phase, next tasks, open questions -> the relevant `phases/Mn.md` + related `docs/` section.
2. Work: one phase at a time. After each unit of work, check the gate with that phase's verification command.
3. End: update `PROGRESS.md` (done checks, evidence, next tasks, new open questions, decision links) -> commit.

### 0.1 Completion criteria
A phase is done only when **all** DoD items are met. If even one falls short, leave it incomplete and do not move to the next phase.

### 0.2 Dependency order
{{M1 -> M2 -> ... -> Mn}}. Even where segments can run in parallel, default to this order. Each phase's entry condition (DoR) names the prerequisite gates.

### 0.3 Evidence-based completion
When claiming a gate "passed," record the **command and key output** (test summary, numbers, a log excerpt) in that `phases/Mn.md`'s evidence section and in `PROGRESS.md`. A completion claim without evidence counts as incomplete.

## 1. File routing
- Phase detail, DoD, verification: `phases/Mn.md` (template: `phases/_TEMPLATE.md`)
- How to write DoD/DoR, archetypes: `gates/DOD_GUIDE.md`
- Invariants: `INVARIANTS.md` (DoD and red lines reference these as INV-n)
- When stuck: `RUNBOOK.md`
- Decision records: `decisions/`
- What to build: `docs/`

## 2. STOP rules
### 2.1 When to stop
- The same error/failure remains unsolved after **3 attempts** via different approaches.
- You can't clear a DoD gate (especially an invariant, core-feature effectiveness, or parity) and feel the urge to work around it.
- Passing would require breaking an invariant (`INVARIANTS.md`).
- A large architectural change appears necessary.
- An external constraint (required resources unavailable, target inaccessible, possible terms/policy violation).
### 2.2 Procedure when stopping
1. Record in `PROGRESS.md`: symptom / how to reproduce / what was tried / hypothesis / where it's stuck.
2. Report the summary to the user, present options if any, and ask for a decision.
3. Until a decision is made, do not build a temporary workaround that breaks an invariant.
### 2.3 Never (red lines)
Same as the red lines in `CLAUDE.md`. In short: faking tests/gates, proceeding with parity or an invariant broken, arbitrarily changing boundaries, committing secrets or large artifacts, weakening guardrails.

## 3. Verification priority (one line)
Invariants / rule correctness > core-feature effectiveness > integration parity > UX/deploy.
Once an earlier gate breaks, later work is meaningless — so always solidify from the front.
