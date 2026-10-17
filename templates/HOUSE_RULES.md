# House rules

Copy this file to the root of a project as `HOUSE_RULES.md` and edit it. Every skill in the `fp` pack reads it first, and it overrides the skills where they disagree.

## Defaults (all projects)

- **Staging first.** Feature branches open PRs into `staging` when the repo has one. `main` only moves by promoting `staging`.
- **No force-push** to `main`, `staging`, or any branch someone else is using. Use `--force-with-lease` only on your own feature branch.
- **Small PRs.** One change per PR, with its own verification. Migrations go in their own PR before the code that needs them.
- **Never skip verification.** Drive the real flow (browser, CLI, API) before calling something done. Type checks and unit tests are not enough. Say what you ran.
- **Merging is not implied.** Opening a PR never means merging it. Wait for an explicit request.
- **Comments** only for a non-obvious why.

## Project: qr-manager (example)

Stack: Next.js on Vercel, Supabase (Postgres, Auth, RLS).

- **Environments.** `staging` branch deploys to STG (Supabase project `xsdossbjumzppcrlayfq`, stg.qrmanager.net). `main` deploys to PROD (Supabase project `slblpnlycutyfrwlcxej`).
- **Merging to `staging` or `main` is a deploy.** Code goes to Vercel and migrations run. Pause and ask for approval before every such merge.
- **Migrations only through the repo.** Add a file under `supabase/migrations/`; the GitHub Action `.github/workflows/supabase-deploy.yml` runs `supabase db push` on push to `staging` and `main`.
- **Never use the Supabase connector's `apply_migration`** or run DDL by hand. It records versions that break `db push`.
- Migrations must be safe on existing PROD data: guard with `if exists` / `if not exists`, backfill before dropping columns, and state the rollback in the PR.
- **Merge method into `main`: rebase only.** No squash or merge commits into `main`.
- **Sync `staging` with `main`** by merging `main` into `staging` with a normal merge. Never reset or force-push `staging`.
- RLS blocks some invitee writes by design; server routes that need them use the admin client. Do not loosen RLS to make a feature work.
- Lessons live in `docs/solutions/`. Read them before planning anything that touches auth, invites, RLS, or migrations.
