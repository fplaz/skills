---
name: tdd
description: Red-green-refactor in vertical slices: one failing behavior test, minimal code to pass, then refactor.
disable-model-invocation: true
---

# TDD

**House rules first.** If `HOUSE_RULES.md` exists at the project root, read it before anything else. Where it conflicts with this skill, it wins.

1. Agree the seams with the user if unclear: which public interface the tests drive. Test behavior through it, never internals.
2. Work in **vertical slices**, one behavior at a time. Do not write all the tests up front.
3. Each slice:
   - **Red.** Write one test for the next behavior. Run it. Watch it fail for the right reason.
   - **Green.** Write the least code that passes. Run the suite.
   - **Refactor.** Clean up while green. No new behavior.
   - Commit.
4. A test that passes on first run proves nothing. Make it fail once.
5. Mock only at system edges (network, clock, payment). Prefer real collaborators and a real test database.
6. Stop when the acceptance criteria are covered. Do not chase coverage numbers.
