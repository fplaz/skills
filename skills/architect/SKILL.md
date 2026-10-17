---
name: architect
description: Settle caller usage, types, and module shape before writing code that crosses a function or module boundary.
disable-model-invocation: true
---

# Architect

**House rules first.** If `HOUSE_RULES.md` exists at the project root, read it before anything else. Where it conflicts with this skill, it wins.

Design the boundary from the caller's side before any implementation.

1. **Write the call site first.** Show two or three real callers using the new API exactly as they would. If the call site is awkward, the design is wrong.
2. **Types next.** Define the inputs, outputs, and error cases as types. Make invalid states unrepresentable. Push validation to the edge.
3. **Module shape.** Where does it live, what does it export, what does it hide? Aim for a deep module: small interface, real work behind it. A pass-through wrapper is a smell.
4. **Explore two or three alternatives** briefly (in parallel subagents if available). A table: shape, caller ergonomics, what it hides, cost to change later.
5. **Red flags:** boolean flag parameters, many optional args, leaking storage details to callers, the same shape assumed in several places, a new layer with one caller.
6. Pick one, say why, and note what you rejected. Then hand back to `feature` or `bug-fix`.

No implementation in this skill beyond signatures and stubs.
