---
name: learn
description: Capture a lesson from a solved problem as a pattern in docs/patterns/<topic>.md (tracked in git) so future work reads it. Use after a fix or feature taught something non-obvious.
disable-model-invocation: true
---

# Learn

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

`docs/patterns/` holds rules learned from real problems. It is not a style guide. Only write lessons that would change a future decision. Skip the obvious.

1. From the conversation, extract the symptom, the root cause, the fix, and the rule that would have prevented it.
2. Pick a topic slug (for example `migrations`, `auth-invites`, `caching`). Check `docs/patterns/` for an existing file on that topic. Update it rather than creating a duplicate. If an older entry is now wrong, fix or remove it.
3. Write or append to `docs/patterns/<topic>.md`:

```
## <short title> (YYYY-MM-DD, `<short sha>`)
**Symptom.** What you saw.
**Cause.** The real mechanism.
**Fix.** What changed. Link the commit or PR if one exists.
**Rule.** One line future work should follow.
**Detect.** How to spot it early (query, log, test).
```

The date and sha (`git rev-parse --short HEAD`) mark when the lesson was true. The code may have moved on since, so readers should check it against the current code.

4. If the repo can enforce the rule (a lint rule, a type, a test, a CI check), propose that instead of or in addition to the note. Structure beats docs.
5. If the rule is a standing project rule, propose adding it to `AGENTS.md`, outside the `fp:start`/`fp:end` block.
6. Print the path. `docs/patterns/` is tracked, so offer to commit it, or to include it in the current branch.
