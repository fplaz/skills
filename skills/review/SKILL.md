---
name: review
description: Review a diff on two axes, spec conformance and standards, then try to break it. No persona swarm. Use before open-pr.
disable-model-invocation: true
---

# Review

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Fix a base point first: the branch base, the PR base, or a commit the user names. Review only `git diff <base>...HEAD`.

**Axis 1: Spec.** Find the origin: the PRD's acceptance criteria, the plan section, or the issue. For each criterion: met, partly, missing, with the line that shows it. Flag scope creep, behavior nobody asked for.

**Axis 2: Standards.** Check against `AGENTS.md`, `HOUSE_RULES.md`, the repo's lint and conventions, and these smells: duplicated logic, long functions doing several things, boolean flag params, primitive obsession, dead code, comments narrating what code says, swallowed errors, missing handling at boundaries.

**Then try to break it.** Think like an attacker and a careless user: nulls, empty lists, concurrent requests, expired or replayed tokens, permissions and RLS, retries, migrations on existing data. Run the code where you can; a claim you can check by running is not a finding until you do.

If a second model or fresh subagent is available, give it the same diff and prompt and compare. Agreement is high signal.

Output: findings grouped as **must fix**, **should fix**, **nit**, each with file:line, evidence, and a suggested fix. Dismiss noise with a reason. Then ask whether to apply the must-fixes.
