---
name: unslop
description: Clean up prose (PR descriptions, docs, replies, commit messages) by removing AI tells and padding.
disable-model-invocation: true
---

# Unslop

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Rewrite the target text. Keep every fact. Cut everything else.

- Short declarative sentences. One idea each.
- No long dashes. Use a period or a comma.
- No throat-clearing ("Great question", "It's worth noting", "In summary").
- No hype words: robust, seamless, leverage, delve, comprehensive, crucial, powerful, elevate.
- No "not just X, but Y" and no groups of three for rhythm.
- Concrete over abstract. Name the file, the number, the user.
- Prefer a plain verb to a noun phrase ("decide", not "make a decision").
- No emoji or bold scattered for emphasis.
- Every claim says how you know: measured, read, or guessed.

Return only the rewritten text, unless the user asks for a diff.
