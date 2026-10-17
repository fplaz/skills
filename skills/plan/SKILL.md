---
name: plan
description: Turn a PRD into an implementation plan: data shape first, one section per PR with unit/live/rollback checks and dependencies. Writes docs/plans/<slug>.md and stops for explicit go.
disable-model-invocation: true
---

# Plan

**House rules first.** If `HOUSE_RULES.md` exists at the project root, read it before anything else. Where it conflicts with this skill, it wins.

The plan is the deliverable. Do not implement.

1. If the change is one or two files with an obvious approach, say so, skip the plan, and suggest `feature` or `bug-fix`.
2. Read the PRD (`docs/prd/<slug>.md`), `docs/glossary.md`, `docs/decisions/`, and **every relevant file in `docs/solutions/`**. List the past lessons that apply and how each changes the plan.
3. Read the code the change touches. Note conventions, test commands, entry points.
4. If a design question can be settled by a quick experiment, run it instead of guessing or asking.
5. **Data shape first.** Name the core types, tables, and state transitions before any step. Prefer a state machine to scattered booleans and a lookup table to branching.
6. Write `docs/plans/<slug>.md`:
   - Summary (under ten lines) and the PRD link.
   - Data shape.
   - One section per PR, in landing order. Each PR is one change with its own evidence. Each section has:
     - Goal and the PRD requirements (FR-n) it covers.
     - Files likely touched.
     - Depends on.
     - `- [ ] unit:` the tests that prove it.
     - `- [ ] live:` how to check it on the real surface (browser on staging, CLI run).
     - `- [ ] rollback:` how to undo it, especially for migrations.
   - Risks and open questions.
7. Prefer more small PRs over one big one. Migrations land in their own PR before code that needs them.
8. Post the path and stop. Start building only on the user's explicit go, one PR at a time with `feature`.
