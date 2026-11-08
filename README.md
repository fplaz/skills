# fp skills

My personal agent skills for Claude Code and Cursor. One `skills/` folder, two manifests.

Most skills are **user-invoked only** (`disable-model-invocation: true`), so they never fire on their own. Two can also be invoked by the agent: `think-deep` (so `create-prd` can call it to refine a rough requirement) and `check-impact` (so planning or `architect` can call it when a shared type or column changes).

## Workflow

```
fp-init (once per project) -> think-deep -> create-prd -> architect -> plan -> build -> learn
                                                          check-impact (any time)
```

Work usually starts from a rough Linear issue or a few lines of text. Mention the issue (`ENG-123` or its URL) when invoking `think-deep` or `create-prd` and they read it and write back to it. Without one, they ask whether to create an issue or write a local file. Without Linear tools, they write the local file.

| Skill | What it does | When to use it |
|---|---|---|
| `fp-init` | Creates `docs/` (only `glossary.md` and `patterns/` tracked), adds the fp docs map, design principles, and working rules to `AGENTS.md` between `<!-- fp:start -->` / `<!-- fp:end -->` markers, and in Claude Code projects sets `plansDirectory` to `docs/plans`. Migrates older setups (`docs/solutions/` to `docs/patterns/`). Safe to re-run; re-running refreshes the block. | Once per project, and again after updating the pack. |
| `think-deep` | Interviews you one question at a time until there are no open gaps. Posts the conclusions as a comment on the Linear issue, or to a new issue or `docs/think-deep-sessions/`. Suggests glossary terms and decisions; writes them only when you approve. | You have a fuzzy idea or a rough issue and want every open question settled. |
| `create-prd` | Product requirements with numbered FRs and acceptance criteria. No implementation. Calls `think-deep` first if the input is too rough. Replaces the Linear issue description with the PRD, or writes a new issue or `docs/prd/<slug>.md`. | You need a reviewable spec before designing or planning the build. |
| `architect` | Design doc in `docs/designs/<slug>.md`: current state, at least two whole-shape alternatives, module map, sketched signatures, data model, and Mermaid flow diagrams. No implementation. Suggests decisions for `docs/decisions/`. | The PRD is settled and the work adds or reshapes modules, so you want the boundaries and contracts agreed before planning. |
| `check-impact` | Finds what a change could break beyond the code it touches, and proves the one fact its safety depends on by running code. Works on a diff, a PR, or a proposed change. | You are about to rename a column, change a shared type or API shape, or tweak a policy, or you have a diff you don't trust. |
| `learn` | Writes the lesson as a pattern in `docs/patterns/<topic>.md` and offers to commit it. | A fix or feature taught you something non-obvious that future work should know. |

Planning and building use your harness's own planning flow, guided by the rules `fp-init` put in `AGENTS.md`. Those rules tell every session to read `docs/patterns/`, `docs/glossary.md`, and `docs/decisions/`, so lessons apply without running a skill.

## Docs are snapshots

Every session conclusion, PRD, design, and pattern is stamped with its date and commit. The codebase is the source of truth: code changes over time and plans change during implementation, so docs are context to verify, not facts to trust.

| Path | Git | Holds |
|---|---|---|
| `docs/glossary.md` | tracked | Project terms. Changed only with your approval. |
| `docs/patterns/` | tracked | Rules learned from real problems, written by `learn`. |
| `docs/decisions/` | local | Hard-to-reverse decisions, added only with your approval. |
| `docs/think-deep-sessions/`, `docs/prd/`, `docs/designs/`, `docs/plans/` | local | Interview conclusions, PRDs, designs, and plans. They do not exist in a fresh clone or a new git worktree. |

## Project rules: AGENTS.md

**Instructions in `AGENTS.md` take precedence over the skills when they conflict.** Put project specifics there: base branch, merge method, deploy rules, how migrations run. Keep your own rules outside the fp markers, because `fp-init` replaces everything between them.

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
- [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every. The compound step that `learn` writes to `docs/patterns/`.

No files were copied verbatim. This pack is MIT licensed.
