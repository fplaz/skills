---
name: learn
description: Record lessons from solved problems in docs/lessons/<YYYY-MM-DD>-<topic>.md (tracked in git) so future work reads them. With a topic ("learn about X"), drafts that lesson. With no input, proposes what is worth recording and lets the user pick. Nothing is written until the user approves the draft.
disable-model-invocation: true
---

# Learn

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

`docs/lessons/` holds rules learned from real problems. It is not a style guide. Only record lessons that would change a future decision. Skip the obvious.

## 1. Pick what to record

- **With a topic** (for example `learn about the invite expiry bug`): use it and go to step 2.
- **No input**: find candidates in this conversation: bugs whose cause was surprising, gotchas, wrong assumptions, approaches that failed. If the conversation has little, also look at the current branch's commits and diff against the base branch. Propose them as a numbered list, one line each: what happened, and the rule it would teach. Ask which to record. If nothing is worth recording, say so and stop.

## 2. Draft

For each chosen lesson:

1. Extract the symptom, the root cause, the fix, and the rule that would have prevented it.
2. Pick a topic slug (for example `migrations`, `auth-invites`, `caching`). Check `docs/lessons/` for an existing file on that topic (`*-<topic>.md`). Prefer updating it over creating a duplicate. If an older entry is now wrong, the draft fixes or removes it.
3. Draft the entry:

```
## <short title> (YYYY-MM-DD, `<short sha>`)
**Symptom.** What you saw.
**Cause.** The real mechanism.
**Fix.** What changed. Link the commit or PR if one exists.
**Rule.** One line future work should follow.
**Detect.** How to spot it early (query, log, test).
```

The date and sha (`git rev-parse --short HEAD`) mark when the lesson was true. The code may have moved on since, so readers should check it against the current code.

A new topic file is named `docs/lessons/<YYYY-MM-DD>-<topic>.md` with today's date and starts with front matter:

```yaml
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
description: <One line: what this topic covers.>
---
```

When appending to an existing file, keep its name and `created`, set `updated` to today, and adjust `description` if the topic grew.

## 3. Approve, then write

Show each draft with its target: the file path, new file or appended, and any older entry it changes or removes. Ask the user to approve, edit, or drop it. Revise until approved. Write only approved drafts.

## 4. Beyond the note

- If the repo can enforce the rule (a lint rule, a type, a test, a CI check), propose that instead of or in addition to the note. Structure beats docs.
- If the rule is a standing project rule, propose adding it to `AGENTS.md`, outside the `fp:start`/`fp:end` block.

Print the paths written. `docs/lessons/` is tracked, so offer to commit it, or to include it in the current branch.
