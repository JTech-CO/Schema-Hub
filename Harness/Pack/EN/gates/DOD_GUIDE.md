# DOD_GUIDE.md — DoR / DoD Authoring Guide + Phase Archetypes

## 1. Definition of Ready (DoR) — entry condition
What must be true before starting a phase. If unmet, do not start.
- The prerequisite phase's DoD gates have passed.
- The needed environment, resources, and access are ready (`docs/ENVIRONMENT`).
- You've reviewed the relevant whitepaper section (`docs/`) and the invariants (`INVARIANTS.md`).
- The input/output contract or schema is defined (where applicable).

## 2. DoD authoring principles
- **It must be measurable.** Not a human eyeballing "seems fine," but confirmed by command output, a number, or a pass/fail boolean.
- **"The code runs" != "the feature works".** Add a gate that observes the effect directly (success rate, accuracy, round-trip match).
- **State invariants separately and cite them as INV-n.** A single violation blocks passing.
- **Parity gates.** When the same logic is implemented in two places, verify numerically that identical inputs produce identical outputs.
- **Verify with reproducible commands.** Anyone running it at any time gets the same verdict.
- **Leave evidence.** When claiming a pass, record the command and key output in the phase file / `PROGRESS.md`.

## 3. Phase archetype menu (pick only what you need; turn it into `phases/Mn.md`)
1. **Foundation / scaffolding** — repo, packages, build/lint/type config, boundary rules. DoD: build/lint/typecheck green; boundary violations blocked by lint (INV-3).
2. **Core domain logic** — pure logic, free of side effects. DoD: unit tests green, determinism, results match known cases.
3. **Data / persistence** — schema, migrations, storage. DoD: migration applies, CRUD round-trip, schema-type consistency.
4. **API / server** — endpoints, contracts, health. DoD: boots + health 200, core endpoints round-trip, schema validation.
5. **Core-feature effectiveness check** ★ — does it actually produce the intended result. DoD: target metric reaches baseline, no divergence/exceptions, artifacts reproduce after save and reload.
6. **Integration / parity** ★ — two implementations/environments agree. DoD: both sides' outputs match for a fixed input set (INV-5).
7. **UI / frontend** — screens, state, real-time. DoD: build green, core flow manual demo, design tokens, no horizontal scroll + basic accessibility.
8. **External integration** — third-party, extensions, OS. DoD: demonstrated against the real target, on change touch one contract file only, guardrails respected.
9. **Benchmark / deploy / CI** — measurement, pipeline, artifacts. DoD: CI green, benchmark report, deploy artifacts.

> Replace the abstract "example DoD" with your project's concrete numbers and commands. (e.g. "target metric reached" -> "Beginner win rate reaches 0.85".)
