---
name: architect
description: Design how a feature or module is shaped before planning it - ground in the current code, weigh at least two whole-shape alternatives, then write docs/designs/<slug>.md with a module map, sketched signatures, data model, and flow diagrams. No implementation. Run after create-prd, before plan mode.
disable-model-invocation: true
---

# Architect

Project rules in `AGENTS.md` (root and nested) and `HOUSE_RULES.md` take precedence over this skill when they conflict.

The design document is the deliverable. Do not write product code, migrations, branches, or commits. The only files you write are `docs/designs/<slug>.md` and any new `docs/decisions/` entries.

This skill decides **how the system is shaped**. It does not restate requirements (that is `create-prd`) or order the work into PRs (that is plan mode). Write for a reader that is an agent who sees only this file and the files it opens.

## 1. Read the inputs

- The PRD in `docs/prd/` if one exists. List every FR. Each must be served by some part of the design. With no PRD, design from the conversation and say so in the Source line.
- `docs/glossary.md`, `docs/decisions/`, and the relevant `docs/solutions/`. Use glossary terms. Respect recorded decisions, or name the one you propose to change.

## 2. Ground in the current code

Explain how every subsystem the design touches works today: its entry points, who owns which data, how calls flow, and what the existing module boundaries are. Cite `path` and `symbol`. Naming a file is not grounding; read it. Use read-only subagents in parallel for breadth when the harness has them. Skip this only for truly greenfield work, and say so.

## 3. Weigh alternatives

Sketch at least two **structurally different** shapes. Different module boundaries, ownership, or data flow count. Point fixes to the same shape do not. For each one, write a short paragraph and a call site showing how a real caller would use it.

Compare them in a table: public surface, what it hides, caller ergonomics, data ownership, failure handling, cost to change later. Prefer the smaller interface that hides more.

Screen every candidate for these red flags:
- A pass-through layer, or a new layer with one caller.
- Callers that must know storage, framework, or ordering rules.
- Boolean flag parameters, or many optional arguments.
- Scattered booleans where a state machine belongs.
- The same shape or rule owned in two places.
- Two writers for the same state with no single owner.
- Escape hatches already needed in the sketch (`any`, casts, always-set optionals).

Pick one and say why. Ask the user only for product or preference calls, one question at a time, with your recommendation. Check facts by reading or running read-only commands instead of asking.

## 4. Write `docs/designs/<slug>.md`

Keep the headings in order. Drop a section only if it truly has nothing.

````markdown
# <Feature> design

Source: `docs/prd/<slug>.md` · Status: draft · <date>

## Summary
<Under ten lines. The shape in plain words, and why it beats the alternatives.>

## Context and constraints
<Constraints from AGENTS.md, HOUSE_RULES.md, decisions, and lessons that shape this design.>

## Current state
<How the affected subsystems work today, with `path` and `symbol`.>

## Options weighed
<One paragraph and a call site per option, then the comparison table and the pick.>

## Module map
```mermaid
flowchart LR
  %% modules and the calls or events between them; mark new vs changed
```
<One line per module: what it owns, what it hides, who calls it.>

## Contracts
<Sketched signatures in the project's language, one block per module. Signatures, types, and errors only. Bodies are `not implemented` or one line of pseudocode.>

```ts
// example shape
export function inviteMember(input: InviteInput): Promise<Result<Invite, InviteError>>
type InviteError = { kind: "already_member" } | { kind: "seat_limit"; limit: number }
```

## Data model
<Entities, fields, ownership, and invariants. Before and after when the schema changes. Add an `erDiagram` when relations change.>

## State
<Only when something has a lifecycle. A `stateDiagram-v2` with every transition and who triggers it.>

## Main flows
<The two or three flows that matter, each as a `sequenceDiagram` plus two or three lines of prose. Include one unhappy path.>

## Failure modes
<What fails, how it is detected, and what the caller or user sees. Retries, idempotency, partial writes, concurrency.>

## Requirement coverage
| FR | Served by (module / contract) |
|---|---|

## Open questions
| Question | Proposed default | Blocks |
|---|---|---|

## Rejected alternatives
<Each option that lost, and why.>
````

Diagram rules: Mermaid only, and only the diagrams that earn their place. Every diagram gets prose next to it, because a planner reads the text. Use the same module names in diagrams, contracts, and prose.

## 5. Record decisions

Each hard-to-reverse choice (a boundary, an ownership rule, a storage choice) goes to `docs/decisions/NNNN-<slug>.md` (context, decision, alternatives rejected, consequences). Number from the highest existing file. Link it from the design.

If a contract changes a shared type, column, config key, or policy, run `check-impact` on the proposed change and put its verdict under Failure modes or Open questions.

## 6. Self-check, then hand back

- Every FR appears in Requirement coverage.
- Every module in the map has a contract block, and every contract belongs to a module in the map.
- No placeholder text left. No implementation beyond stubs.
- `git status` shows no changes outside `docs/` (which is untracked).

Reply with the design path, the chosen shape in two lines, the decisions recorded, and the open questions. Then suggest plan mode to slice it into PRs. If planning or building later runs into repeated friction with this shape, come back here and revise the design instead of patching around it.
