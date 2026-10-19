---
name: help
description: Index of Fernando's skills and the order to use them. Use when unsure which skill fits.
disable-model-invocation: true
---

# Help

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

The workflow, in order:

1. `think-deep`. Get interviewed until the idea has no open gaps.
2. `create-prd`. Write the product requirements. No implementation.
3. `plan`. Turn the PRD into one section per PR. Stops for your go.
4. `implement` or `bug-fix`. Build one PR, test first where cheap, verify on the real surface, self-review. `design-module` is called from inside them.
5. `learn`. Write down what this taught you so the next `plan` reads it.

Situational: `improve-architecture` (find deepening opportunities), `check-blast-radius` (what could a small change break).

Small change of one or two files with an obvious approach? Skip steps 1 to 3.

Look at what the user is trying to do, name the one skill that fits, and give them the exact line to type.
