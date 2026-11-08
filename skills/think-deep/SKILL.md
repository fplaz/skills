---
name: think-deep
description: Interview the user one question at a time until an idea or rough requirement has no open gaps, then record the conclusions on a Linear issue or in docs/think-deep-sessions/. Use only when the user asks for it (think-deep, "grill me", "help me think this through") or when another skill such as create-prd needs to refine a rough requirement. Do not start it on your own for ordinary questions.
---

# Think Deep

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

Goal: shared understanding, not a document. You are done when neither of you can name an open question that changes what gets built.

## Where the output goes

Decide this at the start, but do not ask about it until the end.

- **A Linear issue was mentioned** (an ID like `ENG-123` or a URL) when the skill was invoked or by the calling skill: read its description and comments first. At the end, post the conclusions as a **comment** on it. Never edit the description.
- **Called from another skill** (for example `create-prd`) **with no issue**: write nothing. Hand the conclusions back to that skill.
- **Otherwise**: at the end, ask whether to create a Linear issue (ask which team) or write to `docs/think-deep-sessions/<YYYY-MM-DD>-<slug>.md`. If no Linear tools are available, use the file without asking.

## Interview

1. Read what exists first: the relevant code, `docs/glossary.md`, `docs/lessons/`, earlier `docs/think-deep-sessions/` and `docs/designs/` on the same area, and the Linear issue if there is one. Never ask a question the repo already answers. Say what you found. Treat docs and issues as possibly stale and check claims against the code.
2. Ask **one question per message**. Lead with your recommended answer and why, so the user can just say yes.
3. Walk the design tree depth-first. Settle a branch before opening the next. Track open branches in a short list at the end of each message.
4. Push back. If the premise is wrong, the scope too big, or an answer contradicts an earlier one, say so plainly. Agreement is not the default.
5. If a question is a fact you could check by running or reading something, check it instead of asking.
6. Sharpen vocabulary. When a fuzzy or overloaded word appears, propose one precise term and use it from then on.

## Glossary: suggest, never write unasked

Keep a running list as the interview goes:
- **Glossary suggestions**: new terms or changed definitions, in the glossary's entry shape (`### Term`, then **Means.**, **Not.**, **In code.** if it exists yet). Also flag synonyms (two names for one concept) and overloaded words (one name for two concepts), with a proposed resolution.
At the end, show the list and ask which to apply. Write only the approved ones: terms under `## Terms` in `docs/glossary.md` (alphabetical), open ambiguities under `## Ambiguities`.

Decisions go in the conclusions below, not in a separate file.

## Conclusions

Write them in this shape, as a comment, a new issue, or a file:

```markdown
> Snapshot at commit `<short sha>`. Reference only: the code may have changed since, and plans change during implementation. The codebase is the source of truth.

## Summary
<Five lines: what we are building, for whom, and the key shape of it.>

## Decisions
- <Hard-to-reverse choice, why, and the alternatives rejected.>

## Out of scope
- <What we deliberately won't do.>

## Open questions
- <Question, proposed default, owner.>

## Terms
- <Term: definition. Mark "approved" or "suggested".>
```

Get the sha with `git rev-parse --short HEAD`. When writing to `docs/think-deep-sessions/`, put this front matter above the snapshot line:

```yaml
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
description: <One line: what was settled.>
---
```

Then print where the conclusions went. If you were called from another skill, return to it. Otherwise offer the next step: `create-prd`.
