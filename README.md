# fp skills

My personal agent skills for Claude Code and Cursor. One `skills/` folder, two manifests.

Every skill is **user-invoked only** (`disable-model-invocation: true`), so nothing fires on its own. Type the skill when you want it.

## Workflow

```
think-deep (tt) -> prd -> plan -> feature | bug-fix -> review -> open-pr -> learn
```

| Stage | Skill | What it does |
|---|---|---|
| Before building | `think-deep` / `tt` | Interviews you one question at a time until there are no open gaps. Writes `docs/glossary.md` and `docs/decisions/`. |
| | `prd` | Product requirements in `docs/prd/<slug>.md`. No implementation. |
| | `plan` | Implementation plan in `docs/plans/<slug>.md`. Data shape first, one section per PR with unit, live, and rollback checks. Reads `docs/solutions/`. Stops for your go. |
| Building | `feature` | Understand, name the data shape, checkpoint, small commits, verify on the real surface. |
| | `architect` | Caller usage, types, and module shape before crossing a boundary. |
| | `bug-fix` | Repro first, bisect with instrumentation, failing test, smallest fix. |
| | `tdd` | Red, green, refactor in vertical slices. |
| Quality | `improve-architecture` | Ranked deepening opportunities, then dig into one. |
| | `review` | Spec axis plus standards axis, then try to break it. |
| | `blast-radius` | What else a small change could break, proven by running code. |
| | `unslop` | Strip AI tells from prose. |
| Memory and ship | `learn` | Write the lesson to `docs/solutions/<topic>.md`. |
| | `open-pr` | Why / What changed / Scope / Blast Radius / Verification. Never merges. |
| Index | `help` | Which skill fits right now. |

## House rules

Each skill first reads **`HOUSE_RULES.md` at the project root** if it exists, and it wins over the skill. Start from [`templates/HOUSE_RULES.md`](templates/HOUSE_RULES.md): my defaults plus a qr-manager example section.

```bash
cp ~/path/to/skills/templates/HOUSE_RULES.md ./HOUSE_RULES.md
```

Project docs the skills read and write: `docs/glossary.md`, `docs/decisions/`, `docs/prd/`, `docs/plans/`, `docs/solutions/`.

## Install

### Claude Code

The repo is private, so your git must be able to clone `fplaz/skills` (for example `gh auth login`).

```
/plugin marketplace add fplaz/skills
/plugin install fp@fplaz-skills
```

Or from a terminal: `claude plugin marketplace add fplaz/skills && claude plugin install fp@fplaz-skills`.

Claude Code prefixes plugin skills: type `/fp:think-deep`, `/fp:tt`, `/fp:plan`, and so on. Update with `/plugin marketplace update fplaz-skills`.

Per project instead of globally: install with `--scope project` so it is recorded in `.claude/settings.json`.

### Cursor

Cursor reads `.cursor-plugin/plugin.json`. For a private repo, the reliable path is a local plugin. It must be a real directory under `~/.cursor/plugins/local/`, not a symlink to elsewhere:

```bash
git clone git@github.com:fplaz/skills.git ~/.cursor/plugins/local/fp
# update later
git -C ~/.cursor/plugins/local/fp pull
```

Then run **Developer: Reload Window** and check **Customize** for the skills. Type `/think-deep`, `/tt`, `/plan`, and so on in Agent chat.

Team marketplace import from a private repo also works on Cursor Teams (Dashboard, Settings, Plugins, Import from Repo), but private-repo auth has had bugs. Use the local folder if it fails.

Per project without a plugin: copy chosen skills into the project's `.cursor/skills/` (Cursor) or `.claude/skills/` (Claude Code).

## Why `tt` is its own skill

Neither tool supports skill aliases. `tt` is a two-line skill that tells the agent to read and follow `think-deep`. It works the same in both tools.

## Credits

Ideas adapted, in my own words, from these MIT-licensed projects:

- [pstack](https://github.com/cursor/plugins/tree/main/pstack) by Lauren Tan (poteto), Copyright (c) 2026 Lauren Tan. Feature, Bug fix, architect, interrogate, blast-radius, unslop, multi-phase plan, and the PR format.
- [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock. grill-me / grill-with-docs, to-prd, tdd, improve-codebase-architecture, two-axis code-review.
- [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every. The compound step (`docs/solutions/`) that `learn` and `plan` use.

No files were copied verbatim. This pack is MIT licensed.
