---
name: fp-init
description: Set up a project for the fp skills - create docs/ (only glossary.md and lessons/ tracked), seed the glossary with obvious domain terms and ambiguities from the code (after approval), add the fp docs map, design principles, and working rules to AGENTS.md, and point Claude Code plans at docs/plans. Safe to re-run.
disable-model-invocation: true
---

# fp init

Run from the repository root (`git rev-parse --show-toplevel`). Every step is idempotent: check first, change only what is missing, and never duplicate content.

## 1. docs/

Create whatever is missing:

```
docs/glossary.md             # tracked. Created from glossary-template.md in step 4
docs/lessons/                # tracked (git picks it up once the first file lands)
docs/think-deep-sessions/    # local
docs/prd/                    # local
docs/designs/                # local
docs/plans/                  # local
```

Do not overwrite existing files.

## 2. Track only glossary and lessons

The rules to have in `.gitignore`, in this order:

```
/docs/*
!/docs/glossary.md
!/docs/lessons/
```

1. If `git ls-files docs` lists files outside `docs/glossary.md` and `docs/lessons/`, the project already tracks other docs. Stop this step and ask the user how to proceed. Do not ignore tracked files.
2. Otherwise add whichever of the three lines are missing (create `.gitignore` if needed).
3. Check the result: `git check-ignore -q docs/prd/x.md` must succeed, and `git check-ignore -q docs/glossary.md` and `git check-ignore -q docs/lessons/x.md` must fail.

## 3. AGENTS.md

The block to insert is `agents-section.md`, next to this skill file. Copy it exactly, including the `<!-- fp:start -->` and `<!-- fp:end -->` markers.

- No `AGENTS.md`: create it with the block.
- `AGENTS.md` has the markers: replace everything between them, markers included, with the current block. Leave the rest of the file untouched.
- `AGENTS.md` without the markers: append the block at the end, after a blank line.

Then read the rest of `AGENTS.md`. If an existing rule contradicts a line in the block, do not edit either one. List the conflict for the user. Project rules win.

## 4. Seed the glossary

Only when `docs/glossary.md` is missing or still has no entries under `## Terms`. Otherwise skip seeding, and at most list a few new candidate terms in the report.

1. **Scaffold.** Copy `glossary-template.md`, next to this skill file, to `docs/glossary.md`. Fill in the date and `git rev-parse --short HEAD`.
2. **Find obvious terms.** Look only where the code defines its domain: schema and migrations, models, core types and enums, and roles and permissions. Do not scan the whole repo. Keep:
   - Domain entities (`Lease`, `Unit`, `Invite`).
   - States and status values (`invite.status: pending | accepted | expired`).
   - User roles.

   Skip framework and general technical words (cache, middleware, controller). Skip any term whose meaning the code does not make clear. Aim for 10 to 20 terms. Base each definition on what the code shows, and cite where it is defined.
3. **Find ambiguities.** These matter as much as the terms:
   - **Synonyms**: two names for one concept (`client` in one module, `customer` in another, same table).
   - **Overloaded**: one name for two concepts (`account` meaning a login in one place and a billing entity in another).

   For each one, say where each usage lives and propose a resolution. Never pick silently.
4. **Ask first.** Show the proposed entries in the template's entry shape, then the ambiguities. Ask which entries to add, what to change, and how to resolve each ambiguity. Write only what the user approves:
   - Approved entries go under `## Terms`, alphabetical.
   - A resolved ambiguity becomes, or updates, an entry. The losing name goes in its **Not.** line.
   - An ambiguity the user leaves open goes under `## Ambiguities`.

## 5. Claude Code plans directory

Only if the project uses Claude Code (a `.claude/` folder or `CLAUDE.md` exists, or the user says so): in `.claude/settings.json`, set `"plansDirectory": "docs/plans"`.

- No file: create it with just that key.
- File exists: parse it, add the key, and keep every other key and the formatting style. Never rewrite it as invalid JSON.
- Key already set to a different value: ask before changing it.

## 6. Report

List what you created, changed, and skipped, the glossary terms added and the ambiguities left open, and any conflicts from step 3. Do not commit.
