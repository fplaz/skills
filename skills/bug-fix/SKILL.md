---
name: bug-fix
description: Fix a reported defect - reproduce first on the real surface, bisect the cause with instrumentation, failing test before the fix, smallest change the evidence justifies.
disable-model-invocation: true
---

# Bug Fix

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Every shipped line traces to runtime evidence. A change that "might help" is a hypothesis, not a fix. It does not ship.

1. **Reproduce it yourself** on the surface where it was reported (browser on staging, local app, CLI). Ask the user only if you truly cannot reach that surface, and say why. If it will not reproduce, tighten conditions or add logging until it does.
2. **Bisect the cause.** List candidate hypotheses. Each round, pick the check that rules out the most, run it, and read real output (logs, DB rows, network responses). Add temporary instrumentation rather than guessing. `git bisect` if it is a regression. Check `docs/solutions/` for a known cause.
3. Confirm the mechanism with evidence before fixing. If evidence refutes a hypothesis, revert anything it motivated.
4. **Failing test first** when there is a cheap test path (`tdd`). Commit it before the fix so history shows red then green.
5. **Smallest fix** the evidence justifies. Run `architect` if it crosses a boundary.
6. **Verify on the same surface.** The original repro now passes. Unit tests prove branches, not the absence of the bug. Remove temporary instrumentation.
7. Run `review`, then `open-pr`. Offer `learn` if the cause was surprising.

Reply: what was broken, root cause, the fix, failing-then-passing output.
