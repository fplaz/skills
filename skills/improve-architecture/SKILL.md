---
name: improve-architecture
description: Scan the codebase for deepening opportunities (shallow modules, leaky boundaries, duplication), report them ranked, then dig into the one the user picks.
disable-model-invocation: true
---

# Improve Architecture

**Project rules first.** Read the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files you touch, and `HOUSE_RULES.md` if present. Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over this skill when they conflict.

1. Read `docs/glossary.md` and `docs/decisions/` so findings use the project's language and respect past decisions.
2. Explore the codebase (subagents for breadth if available). Look for:
   - Shallow modules: big interface, little behind it.
   - Logic about one concept scattered across many files.
   - Callers that know storage or framework details they should not.
   - Copy-pasted shapes and validation.
   - Code that is hard to test because the seams are wrong.
3. Report the top five as a ranked list: the problem, files, a proposed deeper module, payoff, cost, risk. Do not change code yet.
4. Let the user pick one. Then use `think-deep` style questioning on it, one question at a time, and finish with a short plan or hand off to `plan`.
5. Record any decision in `docs/decisions/`.
