# Harness Expand Pack — {{Project Name}}

An expanded, advanced, multi-file work harness. Where the single-file harness held only the operational discipline, this pack splits and synthesizes the **agent contract, state handoff, invariants, gates, per-phase detail, decision records, runbook, and whitepapers** into role-specific files. For running complex, long-running projects with Claude Code, it keeps consistency and verification discipline intact even when a session is cut off.

> Convention: fill in `{{...}}` for your project. Lines marked *(guide: ...)* are authoring instructions; delete them once filled.

## How it differs from the single-file harness
- **Separation of concerns**: what changes every session (`PROGRESS.md`) is split from what rarely changes (`HARNESS.md`, `INVARIANTS.md`).
- **Precise references**: you can point the agent at exactly one file (e.g. "for M3 work, read `phases/M3.md` only").
- **Files that grow**: the runbook and decision log accumulate over time, so they live in their own files.
- **Evidence-based completion**: when claiming a gate passed, record the command and output in the phase file / `PROGRESS.md`.

## File map
| File | Role | Change frequency |
|---|---|---|
| `CLAUDE.md` | Root agent contract (auto-loaded). Top-priority rules + index | Rare |
| `HARNESS.md` | Master operating manual: session loop, completion criteria, verification priority, STOP procedure | Rare |
| `INVARIANTS.md` | Invariant registry (INV-n). A single violation blocks passing | Rare |
| `gates/DOD_GUIDE.md` | DoR/DoD authoring principles + phase archetype menu | Rare |
| `phases/_TEMPLATE.md` | Per-phase file template | Fixed |
| `phases/M*.md` | Each phase's entry condition, tasks, DoD, verification, rollback, risks | During work |
| `RUNBOOK.md` | Symptom -> cause -> action (accumulates) | Grows |
| `decisions/_TEMPLATE.md`, `decisions/NNNN-*.md` | Architecture Decision Records (ADR) | On decision |
| `GLOSSARY.md` | Shared vocabulary | Occasional |
| `PROGRESS.md` | Live state handoff. The only file updated every session | Every session |
| `docs/README.md` | Whitepaper set (technical/design/file-tree/environment) definition & skeleton | Early |

## Session protocol (summary — details in `HARNESS.md` §0)
1. Start: `CLAUDE.md` (auto) -> this README -> `PROGRESS.md` -> current `phases/Mn.md` -> relevant `docs/` section.
2. Work: one phase at a time. After each unit of work, check the gate with that phase's verification command.
3. End: update `PROGRESS.md` (done, evidence, next tasks, open questions, decision links) -> commit.

## How to adopt
1. Copy this pack into the repo root (or `harness/`).
2. Fill in `{{...}}` and delete the *(guide: ...)* lines.
3. From `gates/DOD_GUIDE.md` §3 archetypes, pick only the phases you need and instantiate them as `phases/Mn.md`.
4. Register your project's absolute red lines in `INVARIANTS.md` with IDs.
5. Begin the first session.
