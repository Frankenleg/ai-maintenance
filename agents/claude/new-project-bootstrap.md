# New-Project Bootstrap (`/new-project` command)

A custom Claude Code slash command that scaffolds a new project's agent
instruction files in the **`AGENTS.md` canonical + `CLAUDE.md` imports it**
pattern — one source of truth shared by Claude Code and Codex.

## Why a custom command (build, not adopt)

No existing tool does this specific thing: the community generators either wire
`CLAUDE.md → AGENTS.md` via **symlink** (needs admin/Developer Mode on Windows)
or produce a **heavy, codebase-analyzed** `AGENTS.md`. Claude Code's `/init`
only ever emits a standalone `CLAUDE.md`, with no setting to change that. A
small custom slash command is the idiomatic, Windows-safe fit.

## What it produces

- **`AGENTS.md`** — a *light, project-specific* skeleton (overview, stack,
  build/test/run, conventions, gotchas). It deliberately does **not** copy
  global rules (git workflow, etc.) — those flow in from global config, so
  duplicating them would bloat context.
- **`CLAUDE.md`** — just `@AGENTS.md` plus a comment (and room for Claude-only
  overrides).

It refuses to overwrite existing `AGENTS.md`/`CLAUDE.md`.

## Install / location

The live command lives globally at `~/.claude/commands/new-project.md`
(`C:\Users\<you>\.claude\commands\new-project.md`), so it's available in every
project. The version-controlled copy is in
[`commands/new-project.md`](commands/new-project.md) — copy it there to install.

## Usage

```
/new-project
/new-project A CLI for tracking home-lab uptime
```

The optional argument becomes the project's one-line description.

## Related

- [Keeping CLAUDE.md and AGENTS.md in Sync](../../shared/syncing-claude-md-and-agents-md.md)
  — the pattern this command sets up, plus global-level sync.
- [Move Workflows into Script-Driven Skills](script-driven-skills.md)
