---
name: architect
description: Design how a feature or module is shaped before planning it - ground in the current code, weigh at least two whole-shape alternatives, then write docs/designs/<YYYY-MM-DD>-<slug>.md with a module map, sketched signatures, data model, and flow diagrams. No implementation. Takes a PRD, a Linear issue, a drafted document, or a plain description. Run before planning the build.
disable-model-invocation: true
---

# Architect

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

The design document is the deliverable. Do not write product code, migrations, branches, or commits. The only file you write is `docs/designs/<YYYY-MM-DD>-<slug>.md`, plus glossary entries the user approves.

This skill decides **how the system is shaped**. It does not restate requirements (that is `create-prd`) or order the work into PRs (that is planning). Write for a reader that is an agent who sees only this file and the files it opens.

## 1. Read the inputs

- The source: whatever the user hands you. A Linear issue (read its description and comments), a PRD in `docs/prd/`, any other file or drafted document, pasted text, or just the conversation. Name it in the Source line.
- The requirements: list every one the design must serve. A PRD's FRs are used as they are. Any other source: extract the requirements it states or clearly implies, number them `R1`, `R2`, and so on, and show the list to the user before designing. Do not invent requirements. Gaps and unclear points go to Open questions, or ask if they block the design. If the source is too rough to design from, suggest `think-deep` or `create-prd` first.
- `docs/glossary.md`, the relevant `docs/lessons/`, and earlier `docs/designs/` and `docs/think-deep-sessions/` on the same area. Use glossary terms. Respect choices recorded there, or name the one you propose to change.
- Treat the source and docs as snapshots that may be stale. Where they disagree with the code, the code wins; say so in the design.

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

## 4. Write `docs/designs/<YYYY-MM-DD>-<slug>.md`

Use the creation date in the file name. If a design for this slug already exists (`*-<slug>.md`), update it instead: keep its name and `created`, and set `updated` to today. Keep the headings in order. Drop a section only if it truly has nothing.

````markdown
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
description: <One line on what this file covers.>
---

# <Feature> design

Source: <Linear issue, `path`, or "conversation"> · Status: draft

> Snapshot at commit `<short sha>`. Reference only: the code may have changed since, and plans change during implementation. The codebase is the source of truth.

## Summary
<Under ten lines. The shape in plain words, and why it beats the alternatives.>

## Context and constraints
<Constraints from AGENTS.md, earlier designs and sessions, and lessons that shape this design.>

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
| Requirement (FR or R) | Served by (module / contract) |
|---|---|

## Open questions
| Question | Proposed default | Blocks |
|---|---|---|

## Rejected alternatives
<Each option that lost, and why.>
````

Apply `unslop` to the draft before writing it.

Diagram rules: Mermaid only, and only the diagrams that earn their place. Every diagram gets prose next to it, because a planner reads the text. Use the same module names in diagrams, contracts, and prose.

## 5. Glossary and impact

Hard-to-reverse choices (a boundary, an ownership rule, a storage choice) stay in the design, under Options weighed and Rejected alternatives. Make each one explicit: what was chosen, why, and what lost.

Suggest new glossary terms in the glossary's entry shape and ask which to add. Never edit `docs/glossary.md` without approval.

If a contract changes a shared type, column, config key, or policy, run `check-impact` on the proposed change and put its verdict under Failure modes or Open questions.

## 6. Self-check, then hand back

- Every requirement (FR or R) appears in Requirement coverage.
- Every module in the map has a contract block, and every contract belongs to a module in the map.
- No placeholder text left. No implementation beyond stubs.
- No changes outside `docs/designs/` and approved glossary entries.

Reply with the design path, the chosen shape in two lines, the hard-to-reverse choices, and the open questions. Then suggest planning the build: slicing the design into PRs in landing order. If planning or building later runs into repeated friction with this shape, come back here and revise the design instead of patching around it.
