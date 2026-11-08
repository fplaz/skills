---
name: create-prd
description: Write a product/functional PRD like a product manager would, on a Linear issue or in docs/prd/<slug>.md. No implementation detail. Refines rough requirements with think-deep first when needed.
disable-model-invocation: true
---

# Create PRD

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

## 1. Gather the input

- **A Linear issue was mentioned** (an ID like `ENG-123` or a URL): read its description and all comments, including any `think-deep` conclusions.
- Otherwise use the conversation or the text the user gave.
- Also read `docs/glossary.md` and `docs/decisions/`. Treat docs and issues as possibly stale and check claims against the code.

## 2. Refine if it is rough

List the gaps that would change a requirement.
- **One or two gaps**: ask about them yourself, one question at a time, with your recommended answer.
- **More than that**, or the problem or users are unclear: invoke `think-deep` on the input. Pass the Linear issue if there is one. Continue here with its conclusions when it hands back.

Do not re-interview on anything already settled.

## 3. Write the PRD

Use these sections. Drop a section only if it truly has nothing.

```markdown
> Snapshot from <YYYY-MM-DD> at commit `<short sha>`. Reference only: the code may have changed since, and plans change during implementation. The codebase is the source of truth.
```

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
- Reuse terms from `docs/glossary.md`. Suggest new terms to the user; do not edit the glossary without approval.

## 4. Save it

- **Linear issue given**: replace the issue description with the PRD. The original text stays in the issue history.
- **No issue**: ask whether to create a Linear issue (ask which team) or write `docs/prd/<slug>.md`. If no Linear tools are available, use the file without asking.

End by printing where the PRD went. Then suggest the next step: `architect` when the work adds or reshapes modules, or planning the build in PRs when the shape is obvious. Either way, every FR and AC must be covered.
