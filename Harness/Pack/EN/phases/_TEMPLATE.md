# M{{n}} — {{phase name}} {{★ ← mark if this is a core / risky phase}}

**Status**: {{not started / in progress / stuck / done}}  **Updated**: {{YYYY-MM-DD}}

## Context
{{Why this phase is needed — 1-2 lines}}

## Entry condition (DoR)
- [ ] {{prerequisite gate: e.g. "M{{n-1}} DoD passed"}}
- [ ] {{environment / resources ready}}
- [ ] {{relevant whitepaper section & invariants reviewed}}

## Tasks
{{Files/modules to build, in dependency order. a.ts -> b.ts -> ...}}

## References
{{docs/ section numbers, related ADRs (decisions/), invariants (INV-n)}}

## DoD (completion gates)
1. {{a measurable gate — command/number/binary}}
2. {{cite invariant: (satisfies INV-n)}}
3. {{...}}

## Verification
{{command that checks the gate. e.g. pnpm --filter X test, curl ..., inspect a curve/log}}

## Evidence (on pass, paste the command & key output)
~~~
{{command and output summary}}
~~~

## Rollback plan
{{how to undo if this phase goes wrong — commit to revert / migration down / feature flag}}

## Risks / unknowns
- {{uncertain assumptions, possible failure points}}

## Caution
{{common false verdicts, normal ceilings, places where workarounds are forbidden}}
