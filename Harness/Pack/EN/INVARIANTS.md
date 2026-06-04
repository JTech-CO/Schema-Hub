# INVARIANTS.md — Invariant Registry

> An invariant is something that must *never* happen. Unlike ordinary DoD items, **a single violation blocks passing**. DoD items and red lines reference these by ID (INV-n). Add a new invariant with an ID; when retiring one, don't delete the row — mark its status as `Retired`.

## Registration rules
- Write it in a verifiable form (checkable by a command, test, or automated check).
- State in "Scope" which phase/module it applies to.
- Cite it in DoD like `(satisfies INV-3)`.

## Registry
| ID | Invariant | Scope | How verified | Status |
|---|---|---|---|---|
| INV-1 | Secrets (`.env`, keys, tokens) are never committed | Global | pre-commit/CI secret scan, review | Active |
| INV-2 | Large artifacts (weights, binaries, build output) are never committed | Global | `.gitignore`, CI size check | Active |
| INV-3 | No cross-imports that violate module boundaries | Global | eslint boundary rule / lint | Active |
| INV-4 | {{Core correctness invariant: e.g. "zero cases where a cell the solver called 'safe' is actually a mine"}} | {{core logic}} | {{large random-case test}} | Active |
| INV-5 | {{Parity: the two implementations produce identical output for identical input (within tolerance)}} | {{integration point}} | {{fixed input-set parity test}} | Active |
| INV-n | {{...}} | {{...}} | {{...}} | Active |
