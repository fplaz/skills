---
name: create-prd
description: Write a product/functional PRD like a product manager would, saved to docs/prd/<slug>.md. No implementation detail. Run after think-deep, before planning.
disable-model-invocation: true
---

# Create PRD

Project rules in `AGENTS.md` (root and nested) and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Synthesize from the conversation, `docs/decisions/`, and any `think-deep` output. Do not re-interview. Ask only about a gap that would change a requirement, one question at a time.

Write `docs/prd/<slug>.md` with these sections. Drop a section only if it truly has nothing.

1. **Problem.** Who hurts, how, and why now. Two to four sentences.
2. **Users.** Each persona or role, and what they want from this.
3. **Goals / Non-goals.** Non-goals are as important as goals. Name what you are deliberately not doing.
4. **User stories and flows.** "As a <role>, I want <x> so that <y>." Then the main flows step by step, including the unhappy paths.
5. **Functional requirements.** Numbered (FR-1, FR-2...). Observable behavior only.
6. **UX states and edge cases.** Empty, loading, error, permission denied, limits, concurrent edits, already-signed-in, expired links.
7. **Acceptance criteria.** Checkable statements per FR (AC-1.1, AC-1.2...). A plan maps these to PRs, and a reviewer checks the build against them.
8. **Success metrics.** How you will know it worked after launch.
9. **Open questions.** Each with an owner or a proposed default.

Rules:
- No implementation: no database tables or columns, endpoints, libraries, or file names. If you catch yourself writing implementation, move it to a short "Notes for design" list at the end.
- Plain language a non-engineer could review.
- Reuse terms from `docs/glossary.md`.

End by printing the path. Then suggest the next step: `architect` when the work adds or reshapes modules, or plan mode (Shift+Tab in Claude Code) directly when the shape is obvious. Either way, every FR and AC must be covered.
