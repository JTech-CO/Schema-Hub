# {{Project Name}} — Claude Code Work Harness

**Version**: 0.1
**Date**: {{YYYY-MM-DD}}
**Related docs**: root `CLAUDE.md` (global rules & invariants), `PROGRESS.md` (state handoff), whitepapers under `docs/` (technical / design / file-tree / environment, etc.)

> This document defines **not "what to build" (the whitepaper) but "how to proceed, verify, and recover" (operational discipline)**. A phase is "done" not when the code runs but when its **measurable gates (DoD) pass**. The more complex the system, the more common "runs without errors but doesn't actually work" becomes — and if a gate doesn't block it, a wrong "done" verdict propagates into the next phase.

---

<!-- ▶ Template usage note (delete this block once filled in) -->
> **How to use** — Fill in `{{...}}` for your project. Lines marked *(guide: ...)* are authoring instructions; delete them once filled. The number and names of phases (M1, M2, …) differ per project; pick only what you need from the archetype menu in §1.3 and instantiate them as the real phases in §1. **Keep the skeleton** of the gates (DoD), STOP rules (§3), and verification priority (§4) — that skeleton is the actual value of a harness.

---

## 0. Usage

### 0.1 Session loop
1. **Start**: Read `PROGRESS.md` → confirm current phase, next tasks, open questions → review the relevant phase section here + the related whitepaper section.
2. **Work**: One phase at a time. After each unit of work, run that phase's **verification command** and check it against the gate.
3. **End**: Update `PROGRESS.md` (done checks, next tasks, new open questions, decision log) → commit.

### 0.2 Phase completion criteria
- A phase is done only when **all** DoD items are met. If even one falls short, leave it incomplete and do not move to the next phase.
- If you can't clear a gate and feel the urge to work around it → §3 STOP rules.

### 0.3 Dependency order
{{M1 → M2 → … → Mn}}.
Even where segments can run in parallel, default to the order above. Each phase's "entry condition" names the prerequisite gates.

### 0.4 Minimum `PROGRESS.md` structure *(guide: create this handoff file if it doesn't exist)*
Current phase / what was just finished / next tasks / open questions / decision log (date, decision, rationale). Even if a session is cut off, **reading this file alone should be enough to resume work**.

---

## 1. Phases — Entry Condition · Tasks · DoD · Verification

### 1.1 Phase block template *(copy and fill in for each real phase)*

```
### M{{n}} — {{phase name}} {{★ ← mark if this is a core / risky phase}}
- Entry condition: {{prerequisite gates & environment assumptions. If none, "scaffolding complete".}}
- Tasks: {{files/modules to build, in dependency order. Keep it short: a.ts → b.ts → ….}}
- References: {{whitepaper section numbers, etc.}}
- DoD:
  1. {{a measurable gate — a command / number / binary verdict. Not "it runs" but "its behavior is correct".}}
  2. {{state any invariant explicitly: "it is never the case that …".}}
  3. {{...}}
- Verification: {{the actual command that checks the gate. e.g. `pnpm --filter X test`, `curl …`, inspect a curve/log.}}
- Caution: {{common false verdicts, normal ceilings, places where workarounds are forbidden. Omit if none.}}
```

### 1.2 DoD authoring principles
- **It must be measurable.** Not a human eyeballing "seems fine," but a verdict confirmed by a command's output, a number, or a pass/fail boolean.
- **"The code runs" ≠ "the feature works".** Finishing without errors and producing the intended effect are different things. Add a gate that observes the effect directly (e.g. success rate, accuracy, round-trip match).
- **State invariants separately.** Things that must *never* happen (zero false positives, zero data loss, zero privilege escalation, etc.) are kept distinct from ordinary DoD items, and a single violation blocks passing.
- **Parity gates.** When the same logic is implemented in two places (language↔language, client↔server, model↔runtime), verify numerically that identical inputs produce identical outputs. If this breaks, everything downstream is contaminated.
- **Verify with reproducible commands.** Anyone running it at any time should get the same verdict.

### 1.3 Phase archetype menu *(pick only what you need and turn it into a real phase in §1 / example DoDs are illustrative)*

Phase types that recur regardless of project kind. The order generally follows the list below.

1. **Foundation / scaffolding** — repo setup, packages/workspace, build·lint·type config, directory boundary rules.
   - Example DoD: build/lint/typecheck green; boundary violations blocked as lint errors.
2. **Core domain logic** — the pure logic at the heart of the value (testable, free of side effects).
   - Example DoD: unit tests all green; determinism (same input → same output); results match known cases.
3. **Data / persistence** — schema, migrations, storage, ORM.
   - Example DoD: migration applies successfully; CRUD round-trip; schema is consistent with the domain types.
4. **API / server** — endpoints, contracts, health check.
   - Example DoD: boots + health 200; core endpoints round-trip; request/response schema validation passes.
5. **Core-feature effectiveness check** ★ — a gate that checks the thing actually produces the intended result, not just "runs."
   - Example DoD: target metric (success rate / accuracy / latency, etc.) reaches the baseline; no abnormal behavior (divergence/exceptions); artifacts reproduce identically after save and reload.
6. **Integration / parity** ★ — points where two implementations or two environments must agree.
   - Example DoD: for a fixed input set, both sides' outputs match (exactly, or within an allowed tolerance).
7. **UI / frontend** — screens, state management, real-time updates.
   - Example DoD: build green; core flow passes manual demo; design tokens respected; no horizontal scroll + basic accessibility (keyboard, aria, signals beyond color).
8. **External integration** — third-party APIs, browser extensions, OS/hardware integration, and other surfaces we don't control.
   - Example DoD: demonstrated working against the real target; isolated so that a target schema/DOM change is fixed in **one contract file only**; guardrails respected.
9. **Benchmark / deploy / CI** — performance measurement, pipeline, artifacts.
   - Example DoD: CI green (lint/typecheck/test/build); benchmark report generated; deploy artifacts produced.

> **When turning these into real phases**: drop the chosen type into the §1.1 block and replace the abstract "example DoD" with your project's **concrete numbers and commands**. (e.g. "target metric reached" → "Beginner win rate reaches 0.85".)

---

## 2. Runbook (symptom → cause → action)

*(guide: the rows below are generic to almost any project. Record project-specific issues in `PROGRESS.md` with a temporary fix the first time you hit them, and add a row here once they recur.)*

| # | Symptom | Common cause | Action |
|---|---|---|---|
| 1 | Environment / dependency install fails | Version mismatch, wrong source/index, broken lockfile, platform difference | Reinstall per the environment guide, pin versions/sources, regenerate the lockfile |
| 2 | Build / typecheck fails | Type mismatch, config/path error, missing dependency | Narrow down from the error location, check config/paths/dependencies |
| 3 | Flaky tests | Dependence on time/order/external state, unfixed seed, nondeterminism | Fix the seed, isolate external deps (mock), establish determinism |
| 4 | Parity failure | Differing input layout / normalization / ordering between the two implementations | Align both sides to a single source-of-truth doc; compare per-item outputs to pinpoint the divergent part |
| 5 | "Runs but no effect" (core feature) | Missing core signal / wrong scale, unmet precondition, missing input masking/filter | Start with the most common cause (signal, precondition); validate the logic itself via a tiny-case overfit (sanity) |
| 6 | Numeric divergence / blowup (NaN, etc.) | Scale too large, accumulated error, formula error | Check scale/normalization, apply clipping, re-examine the core formula |
| 7 | Slow processing / low resource utilization | Serial bottleneck, unnecessary loops, batching not applied | Parallelize/vectorize, batch processing, profile the hotspots |
| 8 | Boundary-violation lint error | Forbidden cross-import between modules | Respect boundaries; share only through the designated common layer |
| 9 | External integration suddenly breaks | Target API/DOM/schema changed | Update the **contract file only** (don't touch other files), record the change |
| 10 | Behavior differs across environments (local↔CI↔deploy) | Differences in env vars, paths, versions, permissions | Establish environment parity; check env/secrets, paths, versions match |
| 11 | Connection failures (CORS, WS, proxy, etc.) | Missing allowed origin, path mismatch, proxy config | Check allowed origins/paths/proxy config, compare both sides' logs |
| {{n}} | {{project-specific symptom}} | {{...}} | {{...}} |

---

## 3. STOP Rules

### 3.1 When to stop
- The same error / test failure remains unsolved after **3 attempts via different approaches**.
- You can't clear a DoD gate (especially an invariant, core-feature effectiveness, or parity) and feel the urge to work around it.
- Passing would require breaking an invariant (root `CLAUDE.md`).
- A large architectural change appears necessary.
- An external constraint (required resources unavailable, target inaccessible, possible terms/policy violation).

### 3.2 Procedure when stopping
1. Record in `PROGRESS.md`: **symptom / how to reproduce / what was tried / hypothesis / where it's stuck**.
2. Report the summary above to the user, present options if any, and ask for a decision.
3. Until a decision is made, **do not build a temporary workaround that breaks an invariant.**

### 3.3 Never
- Disguise a "pass" by deleting/weakening tests or arbitrarily lowering a gate's numbers.
- Proceed to the next phase with parity or an invariant broken.
- Arbitrarily change architecture or package boundaries (without user approval).
- Commit secrets (`.env`) or large artifacts (weights, binaries, etc.).
- Weaken guardrails (policy violations, excessive requests, excessive permissions).

---

## 4. Verification Priority (one line)
Invariants / rule correctness > core-feature effectiveness > integration parity > UX/deploy.
Once an earlier gate breaks, later work is meaningless — so always solidify from the front.
