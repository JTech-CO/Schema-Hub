# docs/ — Whitepaper Set (what to build)

> If the harness is "how," `docs/` is "what." Each phase reads the relevant whitepaper section before working. As they grow, split them into per-section files (`docs/TECHNICAL.md`, etc.). Below is the minimum skeleton.

## Documents to write
- **Technical (TECHNICAL)** — architecture, data flow, core algorithms, interface contracts.
- **Design (DESIGN)** — UI/UX, design tokens, state & screen flow, accessibility criteria.
- **File tree (FILE_TREE)** — directory/package structure and the **module boundary rules** (who may import whom). The basis for INV-3.
- **Environment (ENVIRONMENT)** — local & CI setup, runtime/dependency versions, secrets & env vars, run/verify commands.

## TECHNICAL skeleton
1. Goals & scope / 2. System composition / 3. Data model & schema / 4. Core algorithms & logic / 5. Interfaces & contracts (I/O) / 6. Points needing parity (INV-5) / 7. Performance & scaling considerations.

## DESIGN skeleton
1. Principles & tone / 2. Design tokens (color, type, spacing) / 3. Screen & state flow / 4. Components / 5. Accessibility (keyboard, aria, signals beyond color) / 6. Forbidden patterns (anti-cliché).

## FILE_TREE skeleton
1. Directory tree / 2. Per-package responsibilities / 3. **Boundary rules table** (allow/deny import) / 4. Shared-layer definition.

## ENVIRONMENT skeleton
1. Prerequisites (runtime & tool versions) / 2. Install steps / 3. Env vars & secrets (.env key list, values empty) / 4. Run commands / 5. Verify commands (test, lint, build) / 6. Resource requirements (where applicable).
