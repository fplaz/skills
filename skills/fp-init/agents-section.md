<!-- fp:start -->
## Project docs

Notes in `docs/`. Read the relevant ones before planning or fixing. Skip any that don't exist.

- `docs/glossary.md` (tracked): project terms. One word, one meaning: use the glossary term, don't introduce a synonym, and don't reuse a term for a different concept. Suggest changes; edit only with the user's approval.
- `docs/patterns/` (tracked): rules learned from real problems. Check before planning a change or diagnosing a bug.
- `docs/decisions/` (local): hard-to-reverse decisions. Respect them, or say plainly that you're proposing to change one. Add new ones only with the user's approval.
- `docs/think-deep-sessions/`, `docs/prd/`, `docs/designs/`, `docs/plans/` (local): interview conclusions, product requirements, module designs, and implementation plans. A plan should follow the design and cover the PRD's acceptance criteria, or say why it departs.

PRDs and interview conclusions may also live on Linear issues.

**The codebase is the source of truth.** Docs and issues are snapshots: the code may have changed since they were written, and plans change during implementation. Treat them as context and verify against the code before relying on them.

## Design principles

- Write the call site before the implementation. If it reads awkwardly, the design is wrong.
- Name the data shape (types, tables, states) before writing logic.
- Prefer deep modules: a small interface with real work behind it. A pass-through wrapper or a new layer with one caller is a smell.
- Make invalid states unrepresentable. Prefer a state machine or enum to scattered booleans, and a lookup table to long branching.
- Validate at the edges, then trust the types inside.
- Keep one concept in one place. If the same shape or rule appears in several files, give it a home.
- Callers shouldn't know storage or framework details.
- No boolean flag parameters and no long lists of optional arguments. Split the function instead.
- Duplication is cheaper than the wrong abstraction. Wait for the third case.
- Comment only a non-obvious why.

## Working rules

- Understand before changing: read the code and say how it works today.
- Bugs: reproduce first, find the cause with evidence (logs, rows, responses, temporary instrumentation), then make the smallest fix the evidence supports. A change that "might help" is a hypothesis, not a fix.
- When there is a cheap test path, write a failing test first and watch it fail for the right reason. Test through the public interface. Mock only at system edges.
- Verify on the real surface: run the app, hit the endpoint, use the CLI. Tests and type checks are not proof. Say what you ran and what you saw.
- Before changing a shared type, column, config key, or policy, find every consumer and prove the change is safe by running something (`check-impact` does this in depth).
- Keep the diff in scope. Remove debug code before handing back.
- Don't push, open PRs, or merge unless asked.
<!-- fp:end -->
