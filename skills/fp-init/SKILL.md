---
name: fp-init
description: Set up a project for the fp skills - create the local docs/ folders (untracked), add the fp design principles and working rules to AGENTS.md, and point Claude Code plan mode at docs/plans. Safe to re-run.
disable-model-invocation: true
---

# fp init

Run from the repository root (`git rev-parse --show-toplevel`). Every step is idempotent: check first, change only what is missing, and never duplicate content.

## 1. docs/

Create whatever is missing:

```
docs/glossary.md     # starts as "# Glossary" plus one line: term, definition, what it is not
docs/decisions/
docs/prd/
docs/designs/
docs/solutions/
docs/plans/
```

Do not overwrite existing files.

## 2. Keep docs/ untracked

1. If `git ls-files docs` lists anything, the project already tracks docs there. Stop this step and ask the user how to proceed. Do not ignore tracked files.
2. Otherwise, if `git check-ignore -q docs/` fails, append `/docs/` to `.gitignore` (create it if needed).

## 3. AGENTS.md

The block to insert is `agents-section.md`, next to this skill file. Copy it exactly, including the `<!-- fp:start -->` and `<!-- fp:end -->` markers.

- No `AGENTS.md`: create it with the block.
- `AGENTS.md` has the markers: replace everything between them, markers included, with the current block. Leave the rest of the file untouched.
- `AGENTS.md` without the markers: append the block at the end, after a blank line.

Then read the rest of `AGENTS.md` (and `HOUSE_RULES.md` if present). If an existing rule contradicts a line in the block, do not edit either one. List the conflict for the user. Project rules win.

## 4. Claude Code plans directory

In `.claude/settings.json`, set `"plansDirectory": "docs/plans"`.

- No file: create it with just that key.
- File exists: parse it, add the key, and keep every other key and the formatting style. Never rewrite it as invalid JSON.
- Key already set to a different value: ask before changing it.

## 5. Report

List what you created, changed, and skipped, along with any conflicts from step 3. Do not commit.
