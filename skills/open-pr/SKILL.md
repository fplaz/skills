---
name: open-pr
description: Open a PR in house format (Why / What changed / Scope / Blast Radius / Verification) respecting the project's branch and merge rules.
disable-model-invocation: true
---

# Open PR

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Pay special attention to their base branch, merge method, and deploy rules.

1. **Branch and base.** Use the base branch the project rules name (default: `staging` if it exists, else `main`). Never force-push a shared branch.
2. **Commits.** Small, ordered, each builds. Rebase locally onto the base if needed. Commit bodies do not repeat the subject.
3. **Title.** Conventional Commits: `type(scope): subject`, imperative, no trailing period.
4. **Body.** A one-minute briefing for a reviewer who has the diff. Use these headings, drop any with nothing to say except Scope:
   - `## Why` problem and approach, one to three sentences.
   - `## What changed` one to three bullets.
   - `## Scope` what it covers and what it deliberately leaves out.
   - `## Blast Radius` who or what it touches and why that is safe or risky.
   - `## Verification` real commands or flows you ran and their outcome. Screenshots if UI.
   Run `unslop` over it.
5. Prefer several narrow PRs to one large one. For a stack, each PR targets its parent branch.
6. Open it ready, not draft, with `gh pr create`. Post the URL.
7. **Do not merge.** Merging is a separate explicit request. If project rules say merging to a branch deploys, pause for approval even when asked to "ship it", and use only the merge method project rules allow.
