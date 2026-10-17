# fp skills

My personal agent skills for Claude Code and Cursor. One `skills/` folder, two manifests.

Every skill is **user-invoked only** (`disable-model-invocation: true`), so nothing fires on its own. Type the skill when you want it.

## Workflow

```
think-deep -> prd -> plan -> feature | bug-fix -> review -> open-pr -> learn
```

| Stage | Skill | What it does | When to use it |
|---|---|---|---|
| Before building | `think-deep` | Interviews you one question at a time until there are no open gaps. Writes `docs/glossary.md` and `docs/decisions/`. | You have a fuzzy idea or a big feature (for example team billing) and want every open question settled before writing anything down. |
| | `prd` | Product requirements in `docs/prd/<slug>.md`. No implementation. | The idea is settled and you need a reviewable spec of what users get, with acceptance criteria, before deciding how to build it. |
| | `plan` | Implementation plan in `docs/plans/<slug>.md`. Data shape first, one section per PR with unit, live, and rollback checks. Reads `docs/solutions/`. Stops for your go. | A PRD exists and the work spans several PRs or touches migrations, so you want the order, checks, and rollbacks agreed first. |
| Building | `feature` | Understand, name the data shape, checkpoint, small commits, verify on the real surface. | You are building one PR of new or changed behavior, usually one section of a plan. |
| | `architect` | Caller usage, types, and module shape before crossing a boundary. | You are about to add a new module, API route, or shared helper that other code will call. |
| | `bug-fix` | Repro first, bisect with instrumentation, failing test, smallest fix. | Something that used to work or should work is broken, for example an invite link failing on staging. |
| | `tdd` | Red, green, refactor in vertical slices. | The behavior has a cheap local test path, such as a pure function, a server action, or an API handler. |
| Quality | `improve-architecture` | Ranked deepening opportunities, then dig into one. | The codebase feels tangled or slow to change and you want to know which refactor pays off most before starting one. |
| | `review` | Spec axis plus standards axis, then try to break it. | A branch is done and you want it checked against its PRD or plan and your standards before opening the PR. |
| | `blast-radius` | What else a small change could break, proven by running code. | You are renaming a column, changing a shared type, or tweaking an RLS policy and want proof nothing else depends on it. |
| | `unslop` | Strip AI tells from prose. | A PR description, README, or user-facing copy reads padded or machine-written. |
| Memory and ship | `learn` | Write the lesson to `docs/solutions/<topic>.md`. | A fix or feature taught you something non-obvious that a future plan should know, such as a migration gotcha. |
| | `open-pr` | Why / What changed / Scope / Blast Radius / Verification. Never merges. | The branch is reviewed and verified and you are ready to open the PR against the right base branch. |
| Index | `help` | Which skill fits right now. | You are not sure which skill to run next. |

## Project rules: AGENTS.md and HOUSE_RULES.md

Every skill starts by reading the project's root `AGENTS.md`, any nested `AGENTS.md` closer to the files being touched, and `HOUSE_RULES.md` if the project has one. **Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over the skill when they conflict.** Put project specifics there: base branch, merge method, deploy rules, how migrations run. `HOUSE_RULES.md` is optional; `AGENTS.md` alone is enough.

Project docs the skills read and write: `docs/glossary.md`, `docs/decisions/`, `docs/prd/`, `docs/plans/`, `docs/solutions/`.

## Install

### Claude Code

The repo is private, so your git must be able to clone `fplaz/skills` (for example `gh auth login`).

```
/plugin marketplace add fplaz/skills
/plugin install fp@fplaz-skills
```

Or from a terminal: `claude plugin marketplace add fplaz/skills && claude plugin install fp@fplaz-skills`.

Claude Code prefixes plugin skills: type `/fp:think-deep`, `/fp:plan`, and so on. Update with `/plugin marketplace update fplaz-skills`.

Per project instead of globally: install with `--scope project` so it is recorded in `.claude/settings.json`.

### Cursor

Cursor reads `.cursor-plugin/plugin.json`. For a private repo, the reliable path is a local plugin. It must be a real directory under `~/.cursor/plugins/local/`, not a symlink to elsewhere:

```bash
git clone git@github.com:fplaz/skills.git ~/.cursor/plugins/local/fp
# update later
git -C ~/.cursor/plugins/local/fp pull
```

Then run **Developer: Reload Window** and check **Customize** for the skills. Type `/think-deep`, `/plan`, and so on in Agent chat.

Team marketplace import from a private repo also works on Cursor Teams (Dashboard, Settings, Plugins, Import from Repo), but private-repo auth has had bugs. Use the local folder if it fails.

Per project without a plugin: copy chosen skills into the project's `.cursor/skills/` (Cursor) or `.claude/skills/` (Claude Code).

## Credits

Ideas adapted, in my own words, from these MIT-licensed projects:

- [pstack](https://github.com/cursor/plugins/tree/main/pstack) by Lauren Tan (poteto), Copyright (c) 2026 Lauren Tan. Feature, Bug fix, architect, interrogate, blast-radius, unslop, multi-phase plan, and the PR format.
- [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock. grill-me / grill-with-docs, to-prd, tdd, improve-codebase-architecture, two-axis code-review.
- [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every. The compound step (`docs/solutions/`) that `learn` and `plan` use.

No files were copied verbatim. This pack is MIT licensed.
