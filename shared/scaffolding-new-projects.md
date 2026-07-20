# Scaffolding New Projects (cross-agent Skills)

Two **Skills** that scaffold a new project's instruction files in the
`AGENTS.md`-canonical + `CLAUDE.md`-import pattern — one behavior spec, used by
**both** Claude Code and Codex.

> **Source of truth: [`ai-skills`](https://github.com/Frankenleg/ai-skills).**
> The skills — `SKILL.md` + a bundled Python `scaffold.py` + tests — live in that
> repo and install from there. This doc records *why* they're shaped the way they
> are and how they fit the rest of this knowledge base; it does **not** copy the
> skill files. For install steps, requirements, and per-skill flow, follow the
> [ai-skills README](https://github.com/Frankenleg/ai-skills#readme).

| Skill | What it does |
| --- | --- |
| **`new-project`** | Creates a light `AGENTS.md` + a `CLAUDE.md` that imports it. **No git.** |
| **`new-git-project`** | `git init` (main) + minimal `.gitignore` + `.gitattributes` (line-ending normalization) + the same `AGENTS.md`/`CLAUDE.md` + an **initial commit**. No remote. |

Each skill is **script-driven**: `SKILL.md` orchestrates while the deterministic
scaffolding runs in a bundled `scaffold.py` (with tests), so behavior can't drift
between runs or agents — the [script-driven-skills](../agents/claude/script-driven-skills.md)
pattern applied in practice. `new-git-project` is **additive and idempotent**: it
creates only what's missing and only commits a fresh repo, so you can run it right
after `new-project` to *add git* to an already-scaffolded project, or re-run it
harmlessly.

## Why Skills, and why two of them

- **Cross-agent, one spec.** A single `SKILL.md` (open [Agent Skills](https://agentskills.io)
  format: `name` + `description` + body) is understood by both tools. This
  replaces the earlier Claude-only `/new-project` *command* — a command and a
  skill doing the same job would drift.
- **Deterministic git behavior.** The git split is deliberate. When git wasn't
  specified, the two agents diverged (Codex ran `git init`, Claude didn't). Now
  the choice is explicit: pick the skill for the behavior you want.

## Install

The skills install into **both** agents' skill directories — Claude Code reads
`~/.claude/skills/<name>/`, Codex reads `~/.codex/skills/<name>/` (or
`$CODEX_HOME/skills/` when set). From a clone of `ai-skills`,
`python scripts/install.py` copies each skill's runtime files (everything except
the tests) into both, idempotently. See the
[ai-skills README](https://github.com/Frankenleg/ai-skills#install) for the
manual-copy path and overrides.

> `~` is your home directory on any OS — `/home/<you>` (Linux), `/Users/<you>`
> (macOS), `C:\Users\<you>` (Windows). Note `~/.agents/skills/` is **not** a
> Codex skills path — that's Codex's *plugin* location
> (`~/.agents/plugins/marketplace.json`), a different concept. Within
> `~/.codex/skills/`, only the `.system/` subdir is reserved (Codex's own
> skills); author yours alongside it.

## Cross-platform

The skills are OS-agnostic: the agent runs the bundled script under whatever OS
it's on. The `new-git-project` `.gitattributes` (`* text=auto`) is the
cross-platform piece — it normalizes line endings so a repo created on one OS
behaves correctly when cloned on another (e.g. Windows ↔ Linux).

## Usage

- **Claude Code:** `/new-project` or `/new-git-project` (or let Claude
  auto-trigger from the description).
- **Codex:** `$new-project` / `$new-git-project`, or pick from `/skills`.

There's no argument-placeholder syntax on the Codex side, so state the project
name/description in your message; the skill passes what it knows to `scaffold.py`
and falls back to tested defaults (name → directory basename) if omitted.

## Gotchas

- **No single shared location** — the same skill installs into two dirs; re-run
  `install.py` from `ai-skills` after a skill changes to update both.
- **Codex Skills** need Codex CLI v0.65+; confirm via `/skills`. Some v0.117.0
  builds dropped skill discovery — restart Codex if a skill doesn't appear.

## Related

- [Move Workflows into Script-Driven Skills](../agents/claude/script-driven-skills.md)
  — the pattern `ai-skills` implements (`SKILL.md` orchestrates, `scaffold.py` does
  the deterministic work).
- [Keeping CLAUDE.md and AGENTS.md in Sync](syncing-claude-md-and-agents-md.md)
  — the pattern these skills set up (and its global-config variant).

## Sources

- [`ai-skills`](https://github.com/Frankenleg/ai-skills) — the skills' source of truth
- https://agentskills.io/specification
- https://learn.chatgpt.com/docs/build-skills
- https://code.claude.com/docs/en/skills.md
