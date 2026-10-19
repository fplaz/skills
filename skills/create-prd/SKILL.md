---
name: create-prd
description: Write a product/functional PRD like a product manager would, saved to docs/prd/<slug>.md. No implementation detail. Run after think-deep, before plan.
disable-model-invocation: true
---

# Create PRD

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Synthesize from the conversation and any `think-deep` output. Do not re-interview. Ask only about a gap that would change a requirement, one question at a time.

Write `docs/prd/<slug>.md` with these sections. Drop a section only if it truly has nothing.

1. **Problem.** Who hurts, how, and why now. Two to four sentences.
2. **Users.** Each persona or role, and what they want from this.
3. **Goals / Non-goals.** Non-goals are as important. Name what you are deliberately not doing.
4. **User stories and flows.** "As a <role>, I want <x> so that <y>." Then the main flows step by step, including the unhappy paths.
5. **Functional requirements.** Numbered (FR-1, FR-2...). Observable behavior only.
6. **UX states and edge cases.** Empty, loading, error, permission denied, limits, concurrent edits, already-signed-in, expired links. Use the project's glossary terms.
7. **Acceptance criteria.** Checkable statements per FR. These become the checks `plan` maps to PRs and `implement` self-reviews against.
8. **Success metrics.** How you will know it worked after launch.
9. **Open questions.** With an owner or a proposed default.

Rules:
- No tables, columns, endpoints, libraries, or file names. If you catch implementation, move it to a note for `plan`.
- Plain language a non-engineer could review.
- Reuse terms from `docs/glossary.md`.

End by printing the path and offering `plan`.
