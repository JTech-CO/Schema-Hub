# M0 — Foundation / Scaffolding (example)

**Status**: example (template demo — replace for your real project)  **Updated**: {{YYYY-MM-DD}}

## Context
Set up the repo, tooling, and boundaries that all later phases depend on. Getting boundaries wrong here causes repeated INV-3 violations.

## Entry condition (DoR)
- [ ] Repository created; package manager & runtime versions decided (`docs/ENVIRONMENT`)
- [ ] Directory / module boundary draft decided (`docs/FILE_TREE`)

## Tasks
Workspace setup -> build/bundle config -> lint/format/type config -> module boundary rules (import restrictions) -> basic CI (build/lint/typecheck).

## References
docs/FILE_TREE (boundaries), INVARIANTS INV-1, INV-2, INV-3.

## DoD (completion gates)
1. `{{build command}}` / `{{typecheck command}}` / `{{lint command}}` all green.
2. A forbidden cross-import is blocked as a lint error (satisfies INV-3) — confirm with a deliberate violation sample.
3. `.gitignore` excludes secrets and large artifacts (satisfies INV-1, INV-2).

## Verification
`{{build}}` && `{{typecheck}}` && `{{lint}}`; confirm lint fails on a boundary-violation sample file, then remove it.

## Evidence (on pass, paste the command & key output)
~~~
{{e.g. tsc --noEmit -> 0 errors / eslint . -> 0 problems / boundary-violation sample -> 1 eslint error confirmed}}
~~~

## Rollback plan
Split scaffolding into separate commits; on a config error, revert only that commit.

## Risks / unknowns
- If boundary rules are too loose, later violations accumulate and surface late.

## Caution
Config that "runs" is not done. Confirm with a sample that the boundary rule **actually blocks** violations.
