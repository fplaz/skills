---
name: feature
description: Build one PR of new or changed behavior: understand first, name the data shape, checkpoint the work, small ordered commits, verify on the real surface.
disable-model-invocation: true
---

# Feature

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

1. **Understand first.** Read the subsystem and explain in a few lines how it works today before changing it. If there is a plan, work only its current PR section.
2. **Name the data shape** before writing logic: types, tables, states. Write it in your reply.
3. If the change crosses a module or function boundary, run `architect` first.
4. **Throughput checkpoint.** Four lines, `n/a: <reason>` allowed:
   - Blocking first steps (migrations, shared types).
   - Independent workstreams that can run in parallel.
   - Shared mutable state, and how you avoid writing it from two places.
   - Smallest safe decomposition.
5. Build. Use `tdd` where there is a cheap local test path. Comment only a non-obvious why.
6. **Verify on the real surface.** Run the app and drive the actual flow (browser, CLI, API). Tests and type checks are not enough. "Inconclusive" is not a pass. Say what you ran and what you saw.
7. Rebase into small ordered commits. Each one builds and tells a step of the story.
8. Run `review`, then `open-pr`.

Reply: what you built, the choices and why, the checkpoint, how you verified, open decisions.
