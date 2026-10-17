---
name: help
description: Index of Fernando's skills and the order to use them. Use when unsure which skill fits.
disable-model-invocation: true
---

# Help

The workflow, in order:

1. `think-deep` (or `tt`). Get interviewed until the idea has no open gaps.
2. `prd`. Write the product requirements. No implementation.
3. `plan`. Turn the PRD into one section per PR. Stops for your go.
4. `feature` or `bug-fix`. Build one PR. `architect` and `tdd` are called from inside them.
5. `review`. Check the diff against the spec and the standards.
6. `open-pr`. Open the PR in the house format.
7. `learn`. Write down what this taught you so the next `plan` reads it.

Situational: `improve-architecture` (find deepening opportunities), `blast-radius` (what could a small change break), `unslop` (clean up prose).

Small change of one or two files with an obvious approach? Skip steps 1 to 3.

**House rules first.** If `HOUSE_RULES.md` exists at the project root, read it before anything else. Where it conflicts with this skill, it wins.

Look at what the user is trying to do, name the one skill that fits, and give them the exact line to type.
