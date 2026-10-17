---
name: blast-radius
description: For a small-looking change, find what else it could break, and prove the safety claim by running code rather than asserting it.
disable-model-invocation: true
---

# Blast Radius

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

1. State the change in one sentence and what you believe makes it safe.
2. Find every consumer: grep for the symbol, string literals, config keys, DB columns, env vars, API routes, generated types, and other repos or jobs that call it. Include migrations, RLS policies, and cron or webhook paths.
3. For each consumer, say what happens after the change: unaffected, behavior change, breaks.
4. Identify the **one fact** the safety depends on (for example "no row has role = null"). Prove it by running something: a query, a test, a script, the app. Paste the output.
5. Verdict: safe, safe with these follow-ups, or not safe. List what to watch after deploy and how to roll back.
