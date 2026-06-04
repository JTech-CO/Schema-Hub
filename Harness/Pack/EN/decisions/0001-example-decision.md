# ADR-0001: Consolidate core logic into a shared core (example)

- **Status**: accepted (example — replace with a real decision)
- **Date**: {{YYYY-MM-DD}}
- **Related**: integration/parity phase, INV-5 (parity)

## Context
Implementing the same core logic separately in two environments (e.g. server/training and client) makes them drift slightly, easily breaking parity (INV-5).

## Decision
Place the core logic (rules, encoding, etc.) in a **single shared core package** that both sides reference. Where differing languages force a reimplementation, force agreement via a fixed input-set parity test.

## Rationale
Duplicate implementations are the root of drift. With a single source of truth, debugging runbook #4 (parity failure) also simplifies to "align to the single source."

## Consequences / trade-offs
- Benefit: parity is easy to maintain; changes happen in one place.
- Cost: an extra package boundary/build, and the cross-language reimplementation carries the upkeep cost of a parity test.

## Alternatives considered
- Independent implementation per environment — fast, but a constant threat to INV-5 via drift. Rejected.
