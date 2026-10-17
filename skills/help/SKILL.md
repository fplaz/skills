---
name: help
description: Index of Fernando's skills and the order to use them. Use when unsure which skill fits.
disable-model-invocation: true
---

# Help

The workflow, in order:

1. `think-deep`. Get interviewed until the idea has no open gaps.
2. `prd`. Write the product requirements. No implementation.
3. `plan`. Turn the PRD into one section per PR. Stops for your go.
4. `feature` or `bug-fix`. Build one PR. `architect` and `tdd` are called from inside them.
5. `review`. Check the diff against the spec and the standards.
6. `open-pr`. Open the PR in the house format.
7. `learn`. Write down what this taught you so the next `plan` reads it.

Situational: `improve-architecture` (find deepening opportunities), `blast-radius` (what could a small change break), `unslop` (clean up prose).

Small change of one or two files with an obvious approach? Skip steps 1 to 3.

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

Look at what the user is trying to do, name the one skill that fits, and give them the exact line to type.
