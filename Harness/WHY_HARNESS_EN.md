# Why a Harness — Using a Structure, Not Just a Prompt or a Skill

Audience: developers, vibe coders, and anyone running real projects with a coding agent like Claude Code.
Basis: every explanation here is grounded in the Schema-Hub harness templates (the single-file version and the multi-file Pack version).

---

## The starting point: "it runs, but the feature doesn't work"

The first line of the harness template gets right to it. A phase is "done" not when the code runs but when its **measurable gates (DoD) pass**. The more complex the system, the more common "runs without errors but doesn't actually work" becomes — and if a gate doesn't block it, that wrong "done" verdict propagates into the next phase.

Vibe coding amplifies this. The agent declares "all set," we take it at its word and move on. The moment verification rests on self-reporting, a wrong "done" accumulates in the codebase. A harness exists precisely to block that.

## What a harness is

A harness defines **not "what to build" (the whitepaper) but "how to proceed, verify, and recover" (operational discipline)**. It is not the content of the work; it is the skeleton of discipline around the work.

The name is literal. Like a climber's safety harness, or a software test harness — it is not the code itself but the frame that holds the code so it gets built correctly. In the template, that skeleton shows up as role-specific components: the session loop, completion criteria (DoD), invariants, STOP rules, verification priority, the runbook, and state handoff.

## The evolution: prompt -> context -> harness engineering

Since AI arrived, "how to use a model well" has developed in three stages.

1. Prompt engineering. The craft of writing a single instruction well. Stateless and one-shot; you tune the wording to coax out the desired output. Fine for short tasks, but it breaks down on long, complex, multi-stage work. When a session is cut off the context is gone, and there is no way to enforce "done."

2. Context engineering. The craft of curating everything in the context window — system prompt, examples, documents pulled via RAG, tool definitions, memory. Stronger than prompting, but still a snapshot of one moment. It governs what is in the window now, but it does not enforce how the agent proceeds across many sessions, what it verifies, or what it must never do.

3. Harness engineering. Structuring the entire operating loop the agent runs inside. Beyond "what is in the window now," it defines a standing process: how this project proceeds, verifies, and recovers across sessions, and what red lines it never crosses. The harness is the durable procedure; the context is what flows through that procedure each session.

The key shift, in one line: "what do I say to the model" -> "what do I put in the window" -> "what is the durable process the agent runs inside."

## How it works (read through the template's structure)

The single-file harness works through these mechanisms.

- Session loop (§0): Start (read `PROGRESS.md` to grasp the current phase, next tasks, open questions) -> Work (one phase at a time; after each unit of work run that phase's verification command) -> End (update `PROGRESS.md`, then commit). This loop is what makes the agent resumable: even if a session is cut off, reading that one file alone should be enough to resume.
- DoD gates (§1): completion must be measurable. Not a human eyeballing "seems fine," but confirmed by a command's output, a number, or a pass/fail boolean. On the premise that "the code runs" is not "the feature works," you add a gate that observes the effect directly (success rate, accuracy, round-trip match).
- Invariants: things that must never happen (zero secrets committed, zero data loss, zero privilege escalation). Kept separate from ordinary DoD items, and a single violation blocks passing. In the multi-file Pack these are registered in `INVARIANTS.md` as INV-n and cited in DoD like `(satisfies INV-3)`.
- Parity gates: when the same logic is implemented in two places (language vs language, client vs server), verify numerically that identical inputs produce identical outputs. If this breaks, everything downstream is contaminated.
- Runbook (§2): a symptom -> cause -> action table. Record an issue the first time you hit it, then promote it to a row once it recurs. It turns recurring pain into a known fix.
- STOP rules (§3): stop when the same failure survives three attempts via different approaches, when you can't clear a gate and feel the urge to work around it, or when passing would require breaking an invariant. On stopping, record symptom / reproduction / what was tried / hypothesis / where it's stuck, and report to the user. And never: fake a "pass" by deleting or weakening tests or arbitrarily lowering a gate's numbers, proceed with parity or an invariant broken, or commit secrets.
- Verification priority (§4): invariants / rule correctness > core-feature effectiveness > integration parity > UX/deploy. Once an earlier gate breaks, later work is meaningless, so always solidify from the front.

The multi-file Pack adds **separation of concerns** on top of this. What changes every session (`PROGRESS.md`) is split from what rarely changes (`HARNESS.md`, `INVARIANTS.md`); you can point the agent at exactly one file ("for M3 work, read `phases/M3.md` only"); and accumulating runbook and decision records (ADRs) live in their own files. `CLAUDE.md` acts as the top-priority contract auto-loaded every session, and `GLOSSARY.md` pins down vocabulary. It also enforces evidence-based completion — when you claim a gate passed, you record the command and output. A completion claim without evidence counts as incomplete.

## How this differs from a prompt or a skill

A prompt is ephemeral. No enforcement, no state across sessions, no recovery procedure. Completion rests on self-reporting. For long, risky projects that is not enough.

A skill is a reusable capability or procedure for a task type (like "how to make a docx"). As an on-demand bundle of "how to do this kind of thing," it is excellent. But a skill is about capability, not about governing a long, multi-stage project's discipline, state, and red lines. If a skill answers "how do I perform this kind of task," a harness answers "how does this whole project proceed, verify, recover, and stay safe across many sessions." They are complementary, not competing — the harness sets the skeleton of discipline, and individual tasks inside it can be performed with skills.

Concretely, the strengths of a harness:

- Resumability: a session can be cut off; reading `PROGRESS.md` alone resumes the work.
- Measurable done: gates block a false "done."
- Safety and red lines: invariants + STOP + a never-list prevent faking passes, weakening tests, committing secrets, and breaking boundaries.
- Recovery: the runbook turns recurring issues into fixes, and the STOP procedure cuts off pointless thrashing.
- Consistency: a glossary + invariants prevent drift across sessions.
- Auditability: evidence-based completion + an ADR decision log preserve the "why" behind choices.
- Scales with complexity: single-file for small projects, the multi-file Pack for long, complex ones.

## When to use which

The single-file harness is for small, short projects. Fill in only what you need, but keep the skeleton of the gates (DoD), STOP rules, and verification priority — that skeleton is the actual value of a harness.

The multi-file Pack is for complex, long-running projects. Separation of concerns, per-phase files, and an accumulating runbook and decision log keep consistency and verification discipline intact even when a session is cut off.

## Bottom line

A prompt decides what the model says right now. A skill gives it a reusable way to perform a task. A harness gives the whole project a durable operating discipline — how to proceed, verify, recover, and never cross the red lines — so that "done" actually means done. The more complex the system, and the less a human touches it, the more decisive that difference becomes.
