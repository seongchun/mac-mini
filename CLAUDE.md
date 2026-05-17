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
