# RUNBOOK.md — Runbook (symptom -> cause -> action)

> The rows below are generic to almost any project. Record project-specific issues in `PROGRESS.md` with a temporary fix the first time you hit them, and promote them to a row here once they recur. For invariant-related symptoms, note the INV-n in the action.

| # | Symptom | Common cause | Action |
|---|---|---|---|
| 1 | Environment / dependency install fails | Version mismatch, wrong source/index, broken lockfile, platform difference | Reinstall per the environment guide, pin versions/sources, regenerate the lockfile |
| 2 | Build / typecheck fails | Type mismatch, config/path error, missing dependency | Narrow down from the error location |
| 3 | Flaky tests | Dependence on time/order/external state, unfixed seed | Fix the seed, isolate external deps (mock), establish determinism |
| 4 | Parity failure (INV-5) | Differing input layout / normalization / ordering between the two implementations | Align both sides to a single source-of-truth doc; compare per-item outputs to pinpoint the divergence |
| 5 | "Runs but no effect" (core feature) | Missing core signal / wrong scale, unmet precondition, missing input masking | Start with signal/precondition; validate the logic via a tiny-case overfit (sanity) |
| 6 | Numeric divergence / blowup (NaN, etc.) | Scale too large, accumulated error, formula error | Check scale/normalization, apply clipping, re-examine the core formula |
| 7 | Slow processing / low resource utilization | Serial bottleneck, unnecessary loops, batching not applied | Parallelize/vectorize, batch processing, profile the hotspots |
| 8 | Boundary-violation lint error (INV-3) | Forbidden cross-import between modules | Respect boundaries; share only through the designated common layer |
| 9 | External integration suddenly breaks | Target API/DOM/schema changed | Update the contract file only (don't touch other files), record the change |
| 10 | Behavior differs across environments (local<->CI<->deploy) | Differences in env vars, paths, versions, permissions | Establish environment parity; check env/secrets, paths, versions match |
| 11 | Connection failures (CORS, WS, proxy, etc.) | Missing allowed origin, path mismatch, proxy config | Check allowed origins/paths/proxy config, compare both sides' logs |
| {{n}} | {{project-specific symptom}} | {{...}} | {{...}} |

## How to add a row
Promote to a row after hitting the same issue 2+ times. Format: symptom (observable) / most common cause first / minimal action. If invariant-related, note the INV-n.
