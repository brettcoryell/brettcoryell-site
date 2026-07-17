
# AGENTS.md — brettcoryell-site

You are a **Codex** agent working as part of Brett Coryell's AI programming team. Claude Code agents may also work in this repository, so keep commits, notes, and architectural decisions explicit enough for another agent to pick up later.

## Agent Roster

- **Claude Code** — runs on imac, mini, or macbook. Session refs: `claude-<machine>-YYYY-MM-DD-topic`.
- **Claude Chat** — Brett's thought-partnership surface. Not a coding agent.
- **Codex** — that's you. Session refs: `codex-<machine>-YYYY-MM-DD-topic`.

## Machine Identity

Run `hostname` to identify yourself. Map to structural `machine` value:
- hostname contains "mini" → `machine=mini`
- hostname contains "MacBook" → `machine=macbook`
- otherwise → `machine=imac`

## Agent Identity and Runtime Context

- Default mode is local Codex app/CLI work on Brett's Mac host.
- Use `source: "Codex"` for OpenBrain context entries. Use `session_ref` format `codex-<machine>-YYYY-MM-DD-topic`.

## Start-of-Session Protocol

1. Run `hostname` and resolve `machine` value.
2. Check repo state: `git status --short` and `git pull --ff-only`.
3. Read this file and then read `DECISIONS.md` if present before non-trivial work.
4. Load OB context on demand:
   - Registry entry: `list_context(topics=["project-registry", "project-brettcoryell-site"], permanent=true, limit=1)`
   - Recent session notes: `list_context(topics=["project-brettcoryell-site"], permanent=false, since="<30-days-ago-ISO>")`

## Branch and PR Policy

- Use `codex/<short-topic>` branch names when creating a separate branch for Codex work.
- PRs are optional, not the default.

## Session-End Protocol

1. Commit all intended changes and push to origin.
2. **Sync tokens**: run `make collect-codex` from `/Users/brettcoryell/Code/AI/token-burn`.
3. Record session context in OpenBrain (fetch-first upsert):
   - Registry: `session_ref: "project-registry-brettcoryell-site"`, permanent, source: "Codex"
   - Session note: `session_ref: "codex-<machine>-<date>-<topic>"`, expires 45 days, source: "Codex"

## Project Snapshot

- Repo: `/Users/brettcoryell/Code/AI/brettcoryell-site`
- GitHub: `brettcoryell/brettcoryell-site`
- Stack: Static personal website with hand-authored HTML/CSS, Vercel deployment, and shared theme primitives.

## Personal Site Rules

- This is a small static site. Keep changes lightweight and inspect the rendered page in desktop and mobile widths for layout regressions.
- Preserve the existing visual tone and theme structure. If borrowing dashboard tokens or theme primitives, keep site-level tokens separate from app-specific `--tb-*` and `--msm-*` tokens.
- Avoid introducing a build system unless Brett explicitly asks for one.
- The site is deployed through Vercel; verify `vercel.json` behavior before changing routing or static assets.

## Networked Git Operations

For local AI repositories, Codex should not attempt networked Git commands inside the sandbox first. Commands such as `git push`, `git pull --ff-only`, `git fetch`, and `git ls-remote` require network access, so use `require_escalated` on the first attempt with a narrow prefix rule when useful.

Do not retry a failed sandboxed Git network command unless the first failure was not network/DNS/remote-access related.

When closing repo work, if a push is needed, run `git push` with escalation on the first try. Brett has approved this pattern for local AI repo handoffs.
