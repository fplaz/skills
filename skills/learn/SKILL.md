---
name: learn
description: Capture a lesson from a solved problem into docs/solutions/<topic>.md so future plans read it. Use after a fix or feature taught something non-obvious.
disable-model-invocation: true
---

# Learn

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Only write lessons that would change a future decision. Skip the obvious.

1. From the conversation, extract: the symptom, the root cause, the fix, and the rule that would have prevented it.
2. Pick a topic slug (for example `supabase-migrations`, `rls`, `auth-invites`). Check `docs/solutions/` for an existing file on that topic. Update it rather than creating a duplicate. If an older entry is now wrong, fix or remove it.
3. Write or append to `docs/solutions/<topic>.md`:

```
## <short title> (YYYY-MM-DD)
**Symptom.** What you saw.
**Cause.** The real mechanism.
**Fix.** What changed, with PR link.
**Rule.** One line a future plan should follow.
**Detect.** How to spot it early (query, log, test).
```

4. If the rule is something the repo can enforce (a lint rule, a type, a test, a CI check), propose that instead of or in addition to the note. Structure beats docs.
5. If the rule is a standing project rule, propose adding it to `AGENTS.md` or `HOUSE_RULES.md`.
6. Print the path. Do not commit unless asked; suggest including it in the current PR.
