---
name: fp-init
description: Set up a project for the fp skills - create docs/ (only glossary.md and patterns/ tracked), add the fp docs map, design principles, and working rules to AGENTS.md, and point Claude Code plans at docs/plans. Migrates older fp setups. Safe to re-run.
disable-model-invocation: true
---

# fp init

Run from the repository root (`git rev-parse --show-toplevel`). Every step is idempotent: check first, change only what is missing, and never duplicate content.

## 1. Migrate an older setup

- `docs/solutions/` exists: move it to `docs/patterns/` (`git mv` if tracked, plain `mv` otherwise). If `docs/patterns/` already exists too, stop and ask how to merge them.
- `.gitignore` has a bare `/docs/` (or `docs/`) line from an older fp-init: remove it. Step 3 adds the new rules.

## 2. docs/

Create whatever is missing:

```
docs/glossary.md             # tracked. Starts as "# Glossary" plus one line: term, definition, what it is not
docs/patterns/               # tracked (git picks it up once the first file lands)
docs/decisions/              # local
docs/think-deep-sessions/    # local
docs/prd/                    # local
docs/designs/                # local
docs/plans/                  # local
```

Do not overwrite existing files.

## 3. Track only glossary and patterns

The rules to have in `.gitignore`, in this order:

```
/docs/*
!/docs/glossary.md
!/docs/patterns/
```

1. If `git ls-files docs` lists files outside `docs/glossary.md` and `docs/patterns/`, the project already tracks other docs. Stop this step and ask the user how to proceed. Do not ignore tracked files.
2. Otherwise add whichever of the three lines are missing (create `.gitignore` if needed).
3. Check the result: `git check-ignore -q docs/prd/x.md` must succeed, and `git check-ignore -q docs/glossary.md` and `git check-ignore -q docs/patterns/x.md` must fail.

## 4. AGENTS.md

The block to insert is `agents-section.md`, next to this skill file. Copy it exactly, including the `<!-- fp:start -->` and `<!-- fp:end -->` markers.

- No `AGENTS.md`: create it with the block.
- `AGENTS.md` has the markers: replace everything between them, markers included, with the current block. Leave the rest of the file untouched.
- `AGENTS.md` without the markers: append the block at the end, after a blank line.

Then read the rest of `AGENTS.md`. If an existing rule contradicts a line in the block, do not edit either one. List the conflict for the user. Project rules win.

## 5. Claude Code plans directory

Only if the project uses Claude Code (a `.claude/` folder or `CLAUDE.md` exists, or the user says so): in `.claude/settings.json`, set `"plansDirectory": "docs/plans"`.

- No file: create it with just that key.
- File exists: parse it, add the key, and keep every other key and the formatting style. Never rewrite it as invalid JSON.
- Key already set to a different value: ask before changing it.

## 6. Report

List what you created, changed, migrated, and skipped, along with any conflicts from step 4. Do not commit.
