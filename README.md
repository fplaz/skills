# fp skills

My personal agent skills for Claude Code and Cursor. One `skills/` folder, two manifests.

## Quick start

1. Install the plugin (see [Install](#install)).
2. In a project, run `fp-init` once. It sets up `docs/`, seeds the glossary, and adds the fp rules to `AGENTS.md`.
3. Start from a rough Linear issue or a few lines of text: `think-deep ENG-123`, or `create-prd ENG-123` if the idea is already clear.

## Workflow

Run `fp-init` once per project, then:

1. `think-deep` (optional): settle a fuzzy idea or rough issue.
2. `create-prd`: write the requirements.
3. `architect` (optional): design the shape when the work adds or reshapes modules.
4. Plan and build: not skills. Use your harness's own planning and build modes. The rules `fp-init` added to `AGENTS.md` guide them and point every session at `docs/lessons/`, `docs/glossary.md`, and earlier sessions and designs.
5. `learn` (optional): record what the work taught you.

Run `check-impact` any time a change touches something shared.

| Skill | What it does | When to use it |
|---|---|---|
| `fp-init` | Sets up the project: `docs/` folders, a seeded glossary, the fp block in `AGENTS.md`, and the Claude Code plans folder. See [What fp-init sets up](#what-fp-init-sets-up). | Once per project, and again after updating the pack. |
| `think-deep` | Interviews you one question at a time until there are no open gaps. Posts the conclusions as a comment on the Linear issue, or to a new issue or `docs/think-deep-sessions/`. Records decisions in its conclusions. Suggests glossary terms and ambiguities, and writes them only when you approve. | You have a fuzzy idea or a rough issue and want every open question settled. |
| `create-prd` | Product requirements with numbered FRs and acceptance criteria. No implementation. Calls `think-deep` first if the input is too rough. Replaces the Linear issue description with the PRD (the original stays in the issue history), or writes a new issue or `docs/prd/<YYYY-MM-DD>-<slug>.md`. | You need a reviewable spec before designing or planning the build. |
| `architect` | Design doc in `docs/designs/<YYYY-MM-DD>-<slug>.md` from a Linear issue or local PRD: current state, at least two whole-shape alternatives, module map, sketched signatures, data model, and Mermaid flow diagrams. No implementation. Records hard-to-reverse choices and the alternatives that lost. | The PRD is settled and the work adds or reshapes modules, so you want the boundaries and contracts agreed before planning. |
| `check-impact` | Finds what a change could break beyond the code it touches, and proves the one fact its safety depends on by running code. Works on a diff, a PR, or a proposed change. | You are about to rename a column, change a shared type or API shape, or tweak a policy, or you have a diff you don't trust. |
| `learn` | Records lessons in `docs/lessons/<YYYY-MM-DD>-<topic>.md`. `learn about <X>` drafts that lesson; plain `learn` proposes what is worth recording and you pick. Shows each draft and writes only after you approve, then offers to commit. | A fix or feature taught you something non-obvious that future work should know. |

**Invocation.** Most skills are user-invoked only (`disable-model-invocation: true`), so they never fire on their own. Two can also be invoked by the agent: `think-deep`, so `create-prd` can call it to refine a rough requirement, and `check-impact`, so planning or `architect` can call it when a shared type or column changes.

**Linear.** Mention an issue (`ENG-123` or its URL) when invoking `think-deep`, `create-prd`, or `architect`, and they read it, including comments. `think-deep` and `create-prd` also write back to it. Without an issue, they ask whether to create one (and in which team) or write a local file. Without Linear tools, they write the local file.

## What fp-init sets up

Every step is safe to re-run. `fp-init` checks first, changes only what is missing, and never commits.

### docs/

| Path | Git | Holds |
|---|---|---|
| `docs/glossary.md` | tracked | Project terms and open ambiguities. Changed only with your approval. |
| `docs/lessons/` | tracked | Rules learned from real problems, written by `learn`. |
| `docs/think-deep-sessions/`, `docs/prd/`, `docs/designs/`, `docs/plans/` | local | Interview conclusions, PRDs, designs, and plans. They do not exist in a fresh clone or a new git worktree. |

The `.gitignore` rules that do this:

```
/docs/*
!/docs/glossary.md
!/docs/lessons/
```

If the project already tracks other files under `docs/`, `fp-init` stops and asks instead of ignoring them.

Every file in `docs/` except `glossary.md` is named `YYYY-MM-DD-<slug>.md` after its creation date and starts with front matter:

```yaml
---
created: 2026-11-08
updated: 2026-11-08
description: Invite flow for team billing.
---
```

Skills set `updated` on every edit and never rename a file when updating it. The convention is also in the `AGENTS.md` block, so ordinary sessions follow it.

### Glossary

One word, one meaning. The glossary has two sections:

- **Terms.** One heading per term, alphabetical:

  ```markdown
  ### Lease
  **Means.** A signed rental agreement between a tenant and an owner for one unit.
  **Not.** An application or a reservation (see Application).
  **In code.** `Lease` model, `leases` table (`app/models/lease.rb`).
  **Status.** seeded
  ```

- **Ambiguities.** Unresolved cases: synonyms (two names for one concept, like `client` and `customer`) and overloaded words (one name for two concepts, like `account` meaning both a login and a billing entity).

On a new project, `fp-init` reads the schema, models, core types, enums, and roles, and proposes 10 to 20 obvious domain terms plus every ambiguity it finds. You approve, change, or resolve each one before anything is written. Approved terms are marked `seeded` until you review them. It seeds only an empty glossary and never overwrites entries.

### AGENTS.md block

A block between `<!-- fp:start -->` and `<!-- fp:end -->` markers, with three parts:

- **Project docs.** The docs map above, the file naming and front matter convention, the glossary approval rule, respecting recorded choices, and the reminder that docs are snapshots.
- **Design principles.** Call site first, data shape first, deep modules, invalid states unrepresentable, validate at the edges, one concept in one place, no boolean flag parameters, and wait for the third case before abstracting.
- **Working rules.** Understand before changing, reproduce bugs with evidence, failing test first when cheap, verify on the real surface, prove shared changes are safe, keep diffs in scope, and don't push, open PRs, or merge unless asked.

Re-running `fp-init` replaces the block with the current version and leaves the rest of `AGENTS.md` untouched. If one of your rules contradicts the block, it lists the conflict instead of editing either one.

### Plans folder (Claude Code only)

In projects that use Claude Code, `fp-init` sets `"plansDirectory": "docs/plans"` in `.claude/settings.json`, so plans land in the repo instead of `~/.claude/plans/`.

### Migrating older setups

`fp-init` moves `docs/solutions/` or `docs/patterns/` to `docs/lessons/` and replaces old ignore lines with the rules above.

## Docs are snapshots

Every session conclusion, PRD, design, and lesson carries its dates in the front matter and the commit it was written at. The codebase is the source of truth: code changes over time and plans change during implementation, so docs are context to verify, not facts to trust. The commit stamp lets you check what changed since, for example `git log <sha>.. -- <path>`.

## Project rules: AGENTS.md

**Instructions in `AGENTS.md` take precedence over the skills when they conflict.** Put project specifics there: base branch, merge method, deploy rules, how migrations run. Keep your own rules outside the fp markers, because `fp-init` replaces everything between them.

## Install

### Claude Code

```
/plugin marketplace add fplaz/skills
/plugin install fp@fplaz-skills
```

Or from a terminal: `claude plugin marketplace add fplaz/skills && claude plugin install fp@fplaz-skills`.

Claude Code prefixes plugin skills: type `/fp:fp-init`, `/fp:think-deep`, `/fp:create-prd`, `/fp:architect`, `/fp:check-impact`, `/fp:learn`.

Per project instead of globally: install with `--scope project` so it is recorded in `.claude/settings.json`.

### Cursor

Cursor reads `.cursor-plugin/plugin.json`. Install it as a local plugin. It must be a real directory under `~/.cursor/plugins/local/`, not a symlink to elsewhere:

```bash
git clone https://github.com/fplaz/skills.git ~/.cursor/plugins/local/fp
```

Then run **Developer: Reload Window** and check **Customize** for the skills. Type `/fp-init`, `/think-deep`, `/create-prd`, `/architect`, `/check-impact`, `/learn` in Agent chat.

On Cursor Teams you can also import it into a team marketplace (Dashboard, Settings, Plugins, Import from Repo) with `fplaz/skills`.

Per project without a plugin: copy chosen skills into the project's `.cursor/skills/` (Cursor) or `.claude/skills/` (Claude Code).

### Updating

1. Pull the new version: `/plugin marketplace update fplaz-skills` in Claude Code, or `git -C ~/.cursor/plugins/local/fp pull` and **Developer: Reload Window** in Cursor.
2. Re-run `fp-init` in each project to refresh the `AGENTS.md` block and apply any migrations.

## Credits

Ideas adapted, in my own words, from these MIT-licensed projects:

- [pstack](https://github.com/cursor/plugins/tree/main/pstack) by Lauren Tan (poteto), Copyright (c) 2026 Lauren Tan. Its architect skill (reworked here as a design-doc `architect`) and blast-radius (now `check-impact`), plus the feature and bug-fix playbooks, condensed into the `AGENTS.md` rules from `fp-init`.
- [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock. grill-me / grill-with-docs (now `think-deep`), to-prd (now `create-prd`), and improve-codebase-architecture (now the design principles).
- [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every. The compound step that `learn` writes to `docs/lessons/`.

No files were copied verbatim. This pack is MIT licensed.
