---
name: think-deep
description: Interview the user one question at a time until a plan or idea has no open gaps, then record decisions and terms in docs. Run before prd.
disable-model-invocation: true
---

# Think Deep

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Goal: shared understanding, not a document. You are done when neither of you can name an open question that changes what gets built.

1. Read what exists first: the relevant code, `docs/glossary.md`, `docs/decisions/`, `docs/solutions/`. Never ask a question the repo already answers. Say what you found.
2. Ask **one question per message**. Lead with your recommended answer and why, so the user can just say yes.
3. Walk the design tree depth-first. Settle a branch before opening the next. Track open branches in a short list at the end of each message.
4. Push back. If the premise is wrong, the scope too big, or an answer contradicts an earlier one, say so plainly. Agreement is not the default.
5. If a question is a fact you could check by running or reading something, check it instead of asking.
6. Sharpen vocabulary. When a fuzzy or overloaded word appears, propose one precise term and use it from then on.
7. When the tree is closed, write:
   - New or changed terms to `docs/glossary.md` (term, one-line definition, what it is not).
   - Each hard-to-reverse decision to `docs/decisions/NNNN-<slug>.md` (context, decision, alternatives rejected, consequences). Skip trivial ones.
8. End with a five-line summary and offer the next step: `prd`.
