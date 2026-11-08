# fp skills

My personal agent skills for Claude Code and Cursor. One `skills/` folder, two manifests.

Every skill except `check-impact` is **user-invoked only** (`disable-model-invocation: true`), so it never fires on its own. Type it when you want it. `check-impact` can also be invoked by the agent, for example from plan mode or `architect` when a shared type or column changes.

## Workflow

```
fp-init (once per project) -> think-deep -> create-prd -> architect -> plan mode -> build -> learn
                                                          check-impact (any time)
```

| Skill | What it does | When to use it |
|---|---|---|
| `fp-init` | Creates the local `docs/` folders (git-ignored), adds the fp design principles and working rules to `AGENTS.md` between `<!-- fp:start -->` / `<!-- fp:end -->` markers, and sets `plansDirectory` to `docs/plans` in `.claude/settings.json`. Safe to re-run; re-running refreshes the block. | Once per project, and again after updating the pack. |
| `think-deep` | Interviews you one question at a time until there are no open gaps. Writes `docs/glossary.md` and `docs/decisions/` as branches close. | You have a fuzzy idea or a big feature and want every open question settled before writing anything down. |
| `create-prd` | Product requirements in `docs/prd/<slug>.md`, with numbered FRs and acceptance criteria. No implementation. | The idea is settled and you need a reviewable spec before planning the build. |
| `architect` | Design doc in `docs/designs/<slug>.md`: current state, at least two whole-shape alternatives, module map, sketched signatures, data model, and Mermaid flow diagrams. No implementation. Records hard-to-reverse choices in `docs/decisions/`. | The PRD is settled and the work adds or reshapes modules, so you want the boundaries and contracts agreed before plan mode slices it into PRs. |
| `check-impact` | Finds what a change could break beyond the code it touches, and proves the one fact its safety depends on by running code. Works on a diff, a PR, or a proposed change. | You are about to rename a column, change a shared type or API shape, or tweak a policy, or you have a diff you don't trust. |
| `learn` | Writes the lesson to `docs/solutions/<topic>.md`. | A fix or feature taught you something non-obvious that future work should know. |

Planning and building use the harness itself: plan mode (Shift+Tab in Claude Code), guided by the rules `fp-init` put in `AGENTS.md`. Those rules tell every session to read `docs/solutions/`, `docs/glossary.md`, and `docs/decisions/`, so lessons apply without running a skill.

## Project rules: AGENTS.md and HOUSE_RULES.md

**Instructions in `AGENTS.md` and `HOUSE_RULES.md` take precedence over the skills when they conflict.** Put project specifics there: base branch, merge method, deploy rules, how migrations run. `HOUSE_RULES.md` is optional. Keep your own rules outside the fp markers in `AGENTS.md`, because `fp-init` replaces everything between them.

`docs/` is local and untracked. It does not travel with the repo, so it will not exist in a fresh clone or a new git worktree.

## Install

### Claude Code

The repo is private, so your git must be able to clone `fplaz/skills` (for example `gh auth login`).

```
/plugin marketplace add fplaz/skills
/plugin install fp@fplaz-skills
```

Or from a terminal: `claude plugin marketplace add fplaz/skills && claude plugin install fp@fplaz-skills`.

Claude Code prefixes plugin skills: type `/fp:fp-init`, `/fp:think-deep`, `/fp:create-prd`, `/fp:architect`, `/fp:check-impact`, `/fp:learn`. Update with `/plugin marketplace update fplaz-skills`.

Per project instead of globally: install with `--scope project` so it is recorded in `.claude/settings.json`.

### Cursor

Cursor reads `.cursor-plugin/plugin.json`. For a private repo, the reliable path is a local plugin. It must be a real directory under `~/.cursor/plugins/local/`, not a symlink to elsewhere:

```bash
git clone git@github.com:fplaz/skills.git ~/.cursor/plugins/local/fp
# update later
git -C ~/.cursor/plugins/local/fp pull
```

Then run **Developer: Reload Window** and check **Customize** for the skills. Type `/fp-init`, `/think-deep`, `/create-prd`, `/architect`, `/check-impact`, `/learn` in Agent chat.

Team marketplace import from a private repo also works on Cursor Teams (Dashboard, Settings, Plugins, Import from Repo), but private-repo auth has had bugs. Use the local folder if it fails.

Per project without a plugin: copy chosen skills into the project's `.cursor/skills/` (Cursor) or `.claude/skills/` (Claude Code).

## Credits

Ideas adapted, in my own words, from these MIT-licensed projects:

- [pstack](https://github.com/cursor/plugins/tree/main/pstack) by Lauren Tan (poteto), Copyright (c) 2026 Lauren Tan. Its architect skill (reworked here as a design-doc `architect`) and blast-radius (now `check-impact`), plus the feature and bug-fix playbooks, condensed into the `AGENTS.md` rules from `fp-init`.
- [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock. grill-me / grill-with-docs (now `think-deep`), to-prd (now `create-prd`), and improve-codebase-architecture (now the design principles).
- [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every. The compound step (`docs/solutions/`) that `learn` writes.

No files were copied verbatim. This pack is MIT licensed.
