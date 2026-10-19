---
name: plan
description: Turn a PRD into a reviewed implementation plan - explore the code, settle open questions with evidence, data shape first, one section per PR with build steps, observable result, and unit/live/rollback checks. Writes docs/plans/<slug>.md and stops for explicit go. Works inside Claude Code or Cursor plan mode.
disable-model-invocation: true
---

# Plan

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

The plan is the deliverable. Do not implement. No product code, no migrations, no branches, no commits. The only file you write is the plan.

## 0. Check the mode

- **Claude Code plan mode** (a system reminder says plan mode is active and names a plan file). The harness blocks every other write. Build the plan in that file with the template below and finish with `ExitPlanMode`, never a text "ok?". After the user approves, first save the same content to `docs/plans/<slug>.md`, then stop and offer `implement` for PR 1. Approval accepts the plan. It is not a go to build unless the user says so.
- **Cursor Plan mode.** Same flow with Cursor's plan file. After the user accepts, save it to `docs/plans/<slug>.md`.
- **Normal mode.** Stay read-only by discipline. Write only `docs/plans/<slug>.md`. For a large or risky change, mention once that plan mode (Shift+Tab) enforces read-only, then continue.

## 1. Gate

1. One or two files with an obvious approach: say so, skip the plan, suggest `implement` or `bug-fix`.
2. No PRD and the work spans several PRs: suggest `create-prd` first. If the user wants to go on, plan from the conversation and say so in the plan's Source line.

## 2. Read the inputs

1. The PRD (`docs/prd/<slug>.md`) in full. List every FR-n and acceptance criterion. Every one must land in the coverage table.
2. `docs/glossary.md` and `docs/decisions/`. Use glossary terms and respect recorded decisions.
3. **Every relevant file in `docs/solutions/`.** Fill the Lessons table, with how each lesson changes this plan.

## 3. Explore the code

1. When the harness has read-only subagents (Claude Code `Explore`, Cursor read-only subagents), run them in parallel, one per area: the subsystem that changes, existing helpers to reuse, tests and test commands, data and migrations. Each returns paths, symbols, conventions, and commands, not file dumps.
2. Then read the critical files yourself. Note entry points, existing functions to reuse (path and symbol), lint, typecheck, and test commands, how migrations run, and the deploy path (staging first, per `AGENTS.md`).

## 4. Settle open questions

1. A question that is a fact (current behavior, a query result, a type, a timing) gets checked by reading or by a read-only command, not asked.
2. If settling it needs writes, add a spike step to the first PR instead of running it now.
3. A product or preference call gets asked, one question at a time, with your recommended default.
4. What stays open goes to Open questions with a default, an owner, and the PR it blocks.

## 5. Choose the approach

1. For non-trivial work, weigh at least two structurally different approaches in a paragraph each. Pick one and say why. The rest go to Appendix A.
2. **Data shape first.** Name the core types, tables, columns, and state transitions before any PR. Prefer a state machine to scattered booleans and a lookup table to branching. Show before and after when a schema changes.

## 6. Slice into PRs

- One PR is one change with its own evidence, in landing order.
- Migrations land in their own PR before the code that needs them. A destructive change goes expand, migrate, contract, each in its own PR.
- No PR leaves the base branch broken or exposes a half-built feature. Hide it behind a flag or an unreachable route when needed.
- Prefer more small PRs over one big one. Mark which PRs can run in parallel.

## 7. Write `docs/plans/<slug>.md`

Copy this skeleton, keep the headings in order, and fill every placeholder. Drop a section only if it truly has nothing, and write "None" rather than deleting a PR sub-block.

````markdown
# <Feature> plan

Source: `docs/prd/<slug>.md` · Status: draft · <date>

## Summary

<Under ten lines. What changes, for whom, and the PR ids in order.>

## Context and constraints

<Why now. Constraints from AGENTS.md and HOUSE_RULES.md, such as base branch, staging first, how migrations run, who merges.>

## Lessons applied

| Source | Lesson | How it changes this plan |
|---|---|---|

## Current state

<Entry points, key files, existing helpers to reuse as `path` and `symbol`, test, lint, and typecheck commands.>

## Approach

<The chosen approach and why, in one or two paragraphs.>

## Data shape

<Types, tables, columns, states, and transitions. Before and after.>

## Requirement coverage

| FR / AC | PR | Check that proves it |
|---|---|---|

## PRs

### PR 1 - <task as a verb phrase>

**Covers.** <FR-n, AC-n>
**Depends on.** <PR id, or None>
**Files.** <`path` edit, create, or delete>

**Build.**
- [ ] <One change. Name the file and the symbol.>

**You see.** <The observable result: screen state, response, or log line.>

**Verify.**
- [ ] unit: <test file and case>. Run `<command>`.
- [ ] live: <scenario on staging or local, the steps, and "pass when ...">.
- [ ] perf: <only when the PRD names a performance target; metric, baseline, failing number>.
- [ ] rollback: <how to undo. For migrations, the down path and whether data survives.>

**Risk.** <What else this could break. Run `check-blast-radius` if shared types, columns, or policies change.>

## Test strategy

<Unit, integration, and end-to-end mix. Fixtures or seed data. Existing flows to regression-check. What is not tested and why.>

## Rollout

<Staging then production. Flags, migration order, backfills, what to monitor after release.>

## Risks

| Risk | Impact | PR | Mitigation or what to watch |
|---|---|---|---|

## Open questions

| Question | Proposed default | Owner | Blocks |
|---|---|---|---|

## Out of scope

<What this plan deliberately does not do.>

## Appendix A. Alternatives rejected

<Each approach weighed and why it lost.>

## Appendix B. Evidence

<What each check in step 4 showed, with the command or file. What stays unproven.>
````

## 8. Self-check before handing back

- Every FR-n and acceptance criterion appears in Requirement coverage, and every row names a PR and a check.
- Every PR has Covers, Depends on, Files, at least one Build box naming a file and symbol, You see, unit, live, and rollback.
- Every migration is in its own PR with a rollback.
- Every applicable lesson from `docs/solutions/` shows up in a PR or a risk.
- No placeholder left. No "TBD" without a default and an owner.
- Nothing implemented. `git status` shows only the plan file.

Fix what fails before you reply.

## 9. Hand back

Post the plan path, the PRs in order with dependencies, the open questions that need the user, and what stays unproven. Then stop. In plan mode, ask for approval with `ExitPlanMode`. Build only on the user's explicit go, one PR at a time with `implement`, and tick the plan's boxes as each piece of evidence exists.
