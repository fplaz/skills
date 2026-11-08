---
name: check-impact
description: Find what a change could break beyond the code it touches, and prove the one fact its safety depends on by running real code. Use before or after changing a shared type, function signature, DB column, config key, env var, API shape, or access policy, when reviewing a diff you don't trust, or when the user asks "what could this break" or "is this safe".
---

# Check Impact

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

Listing the callers is not the job. The job is the breakage grep won't show, and proof that the change is safe rather than a writeup that says so.

**Input.** One of: the current diff (`git diff` against the base branch), a PR, or a proposed change described in words. For a proposed change, reason about the code as it is now.

## Steps

1. **State the change.** Name the symbols, columns, keys, or shapes added, changed, or removed, and any behavior change the diff doesn't spell out.
2. **Find the one fact** the change is safe because of, for example "no row has `role = null`" or "every caller already passes a non-empty list". If it holds, most risks clear at once. Spend your effort here, not on a long list of maybes. Sometimes there are two facts. Rarely more.
3. **Look where grep stops.**
   - Non-symbol paths: DB columns and views, JSON or wire formats, string keys, env vars, feature flags, generated types, routes and webhooks, cron and queue jobs, other repos or services that read the same data.
   - Timing: when things run (startup, teardown, retries, concurrent writers, cached values that outlive the change).
   - Libraries: read the source of the version actually installed, not the docs.
   - Existing data: rows, cache entries, and stored payloads written under the old shape.
4. **Weigh each risk honestly.** Give it a real likelihood and a real cost. Cite `file:line`. A search that finds nothing is still an answer, so say what you searched. Never invent a caller or an API.
5. **Prove the fact.** Write a small script, query, or test that exercises the real code or data and fails loudly if the fact is wrong. Run it and paste the output. Run proofs locally or on staging. Against production, only light read-only queries, and only if the user allows it. Keep proof scripts out of the diff unless the user wants them as tests.

**Confidence ladder.** For each fact, go as far up as is cheap and say where you stopped:
1. You said so (worthless alone).
2. You pointed at the line.
3. You walked the failure step by step and showed it cannot happen.
4. You ran code that would fail if the fact were wrong.
5. You reproduced it in the running app.

## Reply

- **Change.** What it does, including the non-obvious part.
- **Safety fact.** The fact, the ladder step reached, and the proof output. Write "unproven" if you did not reach step 4.
- **Risks.** Confirmed risks only: how it breaks, `file:line`, likelihood and severity, how to check.
- **Cleared.** What you checked and why it is fine.
- **Before merge.** The cheapest test or repro that would catch the real bug, and what to watch after deploy and how to roll back.
- **Verdict.** Safe, safe with follow-ups, or not safe.
