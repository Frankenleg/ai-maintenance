# AI Maintenance

A personal, evolving knowledge base for **managing and maintaining AI coding
agents** — primarily [Claude Code](agents/claude/) and Codex.

## Purpose

Coding agents are powerful but easy to run inefficiently — bloated context,
the wrong model on the wrong task, unvetted third-party tools. This repo
captures, in one place:

- **Techniques** for keeping agents fast, cheap, and well-configured — context
  and token management, model routing, script-driven skills, cross-agent
  workflows.
- **Evidence-based tool evaluations** — research done *before* adopting a
  plugin or tool, with a clear verdict and sources, so decisions are recorded
  and revisitable.

Everything here is verified against official docs or first-hand testing rather
than transcribed uncritically (see [Conventions](#conventions)).

## Organization

- **[`agents/`](agents/)** — docs specific to a single agent.
  - **[`agents/claude/`](agents/claude/)** — Claude / Claude Code techniques,
    setup, and tool evaluations.
- **[`shared/`](shared/)** — cross-cutting docs (and cross-agent Skills) that
  apply to any agent — orchestrating multiple agents, syncing instruction files,
  scaffolding new projects.

Each folder has its own `README.md` index. If a doc needs images, add them in an
`images/` subfolder beside it. If a topic starts agent-specific and later proves
cross-cutting, it moves into `shared/` and the links are updated.

## Related repositories

- **[`ai-skills`](https://github.com/Frankenleg/ai-skills)** — the canonical home
  for the cross-agent scaffolding Skills (`new-project`, `new-git-project`),
  built as script-driven Python skills with tests. This repo documents and links
  to them; `ai-skills` owns the code.

## Start here

- **Trim what loads every session** →
  [Clean Up Session-Start Context](agents/claude/clean-up-session-start-context.md)
- **Spend less per task** →
  [Reduced Models for Subagents & Skills](agents/claude/reduced-models-for-subagents-and-skills.md)
- **Use Claude Code + Codex together** →
  [Orchestrating Claude Code and Codex](shared/orchestrating-claude-and-codex.md)
- **Share instructions across agents** →
  [Keeping CLAUDE.md and AGENTS.md in Sync](shared/syncing-claude-md-and-agents-md.md)
- **Set up a new project** →
  [Scaffolding New Projects](shared/scaffolding-new-projects.md)
  (`new-project` / `new-git-project` skills)

Full lists live in the [Claude index](agents/claude/README.md) and the
[shared index](shared/README.md).

## Conventions

Rules for contributing to this repo live in [`AGENTS.md`](AGENTS.md) (the
canonical instruction file). In short: one doc per topic, verify before
documenting, copy-paste items in code blocks, cross-link related docs, and keep
the indexes current.

This repo also practices what it documents — its own agent instructions use the
`AGENTS.md`-canonical + `CLAUDE.md`-import pattern described in
[Keeping CLAUDE.md and AGENTS.md in Sync](shared/syncing-claude-md-and-agents-md.md).
