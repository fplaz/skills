---
name: blast-radius
description: For a small-looking change, find what else it could break, and prove the safety claim by running code rather than asserting it.
disable-model-invocation: true
---

# Blast Radius

**House rules first.** If `HOUSE_RULES.md` exists at the project root, read it before anything else. Where it conflicts with this skill, it wins.

1. State the change in one sentence and what you believe makes it safe.
2. Find every consumer: grep for the symbol, string literals, config keys, DB columns, env vars, API routes, generated types, and other repos or jobs that call it. Include migrations, RLS policies, and cron or webhook paths.
3. For each consumer, say what happens after the change: unaffected, behavior change, breaks.
4. Identify the **one fact** the safety depends on (for example "no row has role = null"). Prove it by running something: a query, a test, a script, the app. Paste the output.
5. Verdict: safe, safe with these follow-ups, or not safe. List what to watch after deploy and how to roll back.
