---
name: think-deep
description: Interview the user one question at a time until a plan or idea has no open gaps, recording decisions and terms in docs/ as they settle. Run before create-prd.
disable-model-invocation: true
---

# Think Deep

Project rules in `AGENTS.md` (root and nested) and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Goal: shared understanding, not a document. You are done when neither of you can name an open question that changes what gets built.

1. Read what exists first: the relevant code, `docs/glossary.md`, `docs/decisions/`, `docs/solutions/`. Never ask a question the repo already answers. Say what you found.
2. Ask **one question per message**. Lead with your recommended answer and why, so the user can just say yes.
3. Walk the design tree depth-first. Settle a branch before opening the next. Track open branches in a short list at the end of each message.
4. Push back. If the premise is wrong, the scope too big, or an answer contradicts an earlier one, say so plainly. Agreement is not the default.
5. If a question is a fact you could check by running or reading something, check it instead of asking.
6. Sharpen vocabulary. When a fuzzy or overloaded word appears, propose one precise term and use it from then on.
7. **Write as branches close**, not only at the end, so nothing is lost if the session runs long:
   - New or changed terms go to `docs/glossary.md` (term, one-line definition, what it is not).
   - Each hard-to-reverse decision goes to `docs/decisions/NNNN-<slug>.md` (context, decision, alternatives rejected, consequences). Number from the highest existing file. Skip trivial decisions.
8. When the tree is closed, end with a five-line summary and offer the next step: `create-prd`.
