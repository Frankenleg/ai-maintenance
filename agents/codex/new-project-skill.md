# New-Project Skill (Codex)

A Codex **Skill** that scaffolds a new project's instruction files in the
`AGENTS.md`-canonical + `CLAUDE.md`-import pattern — the Codex counterpart to
Claude Code's [`/new-project` command](../claude/new-project-bootstrap.md). Same
output (a light `AGENTS.md` + a `CLAUDE.md` that imports it), so a project set up
by either tool works for both.

## Why a Skill (not a custom prompt)

Codex's custom prompts (`~/.codex/prompts/`) are **deprecated** in favour of
**Skills**, which is the durable, officially-recommended path. Skills also use
the cross-tool **open Agent Skills standard**, so this same `SKILL.md` folder is
portable to Claude Code's `.claude/skills/` too.

## Location

- **User scope:** `~/.agents/skills/new-project/SKILL.md`
  (Windows: `C:\Users\<you>\.agents\skills\new-project\SKILL.md`).
- **Project scope (optional):** `<repo>/.agents/skills/new-project/SKILL.md` —
  Codex also discovers skills from `.agents/skills` inside a repo.
- Note: `~/.codex/skills/` is **reserved** for OpenAI's bundled system skills —
  don't author here.

The version-controlled copy lives in
[`skills/new-project/SKILL.md`](skills/new-project/SKILL.md) — copy that folder
into `~/.agents/skills/` to install.

## Format

Open Agent Skills `SKILL.md`: a folder with a `SKILL.md` whose YAML frontmatter
has just `name` and `description`. The **`description` is the trigger** — Codex
preloads it and auto-selects the skill when a task matches.

There is **no `model:` field** (Codex sets the model at the session level via
`/model` or a profile) and **no argument-placeholder syntax** — the skill reads
the project name/description from the conversation, and asks if it's missing.

## Usage

In an interactive Codex session:

```text
$new-project
```

or pick it from the `/skills` menu, or just describe the task and let Codex
auto-trigger it (e.g. "start a new project called Foo, a CLI for bar"). Provide
the name/description in the message since there's no argument syntax.

## Requirements & gotchas

- Skills shipped in **Codex CLI v0.65.0** (Dec 2025) and are still described as
  experimental — treat v0.65+ as the floor.
- Codex detects skill changes automatically; **restart Codex** if a new skill
  doesn't appear.
- Known regression: some v0.117.0 builds dropped skills discovery — after
  installing, confirm it shows up via `/skills`.

## Related

- [New-Project Bootstrap (`/new-project`)](../claude/new-project-bootstrap.md) —
  the Claude Code counterpart.
- [Keeping CLAUDE.md and AGENTS.md in Sync](../../shared/syncing-claude-md-and-agents-md.md)
  — the pattern this skill sets up.

## Sources

- https://learn.chatgpt.com/docs/build-skills
- https://learn.chatgpt.com/docs/custom-prompts (deprecation note)
- https://github.com/openai/skills
