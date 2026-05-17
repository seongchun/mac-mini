# Repo notes for Claude

## Cross-repo sync with `seongchun/victus`

This repo (`seongchun/mac-mini`) and `seongchun/victus` are siblings in the
phone → cloud → home-machine dispatch flow. Keep their Claude-facing
configuration in sync unless the user explicitly asks for a divergence.

Files that should mirror between the two repos:

- `.claude/settings.json` (permissions, hooks, `env` block)
- `.claude/hooks/` (especially `session-start.sh`)
- `.claude/commands/` (skill definitions used from either side)
- `.env.example` (shared variable names and public values)
- `scripts/` (dispatch/status/sessions/logs/cancel/new-session)
- This `CLAUDE.md`

When changing any of the above here, also apply the equivalent change in
`seongchun/victus` (and vice versa). If repo-specific behavior is genuinely
required, call it out in the diff so the divergence is intentional.

## Shared cloud environment

Both repos are designed to be used with a single Claude Code web
**environment** that injects `SUPABASE_URL`, `SUPABASE_ANON_KEY`, and
`DISPATCH_SECRET`. The first two are also inlined in `.claude/settings.json`
as a fallback; `DISPATCH_SECRET` must live in the web environment config
(or a local `.env`) since it is a real secret and must match the value the
Mac mini relay is running with.

## Branch convention: `dispatch/<workspace>`

Every task tied to a relay workspace runs on a branch named
`dispatch/<workspace>`, where `<workspace>` is the same identifier used for
the Mac/Victus workspace directory and for `sessions` listings. The
workspace name is the single source of truth that joins cloud sessions,
the home-machine workspace dir, and the git branch.

Rules for Claude:

- When starting work in a workspace (whether on cloud or a home machine),
  check out or create `dispatch/<workspace>` before the first commit. If
  the harness has auto-assigned a different branch (e.g. `claude/...`),
  rename it: `git branch -m dispatch/<workspace>`.
- Don't encode the device name in the branch. A task that migrates between
  mac-mini, victus, and cloud keeps the same `dispatch/<workspace>` branch
  — device identity lives in commit metadata and `sys_log`, not in the
  branch name.
- One-off cloud sessions that aren't tied to a relay workspace (e.g.
  ad-hoc fixes to this repo) may keep the harness-assigned `claude/...`
  branch.
