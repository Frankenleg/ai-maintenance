# Scaffolding New Projects (cross-agent Skills)

Two **Skills** that scaffold a new project's instruction files in the
`AGENTS.md`-canonical + `CLAUDE.md`-import pattern — one behavior spec, used by
**both** Claude Code and Codex.

| Skill | What it does |
| --- | --- |
| **`new-project`** | Creates a light `AGENTS.md` + a `CLAUDE.md` that imports it. **No git.** |
| **`new-git-project`** | `git init` (main) + minimal `.gitignore` + `.gitattributes` (line-ending normalization) + the same `AGENTS.md`/`CLAUDE.md` + an **initial commit**. No remote. |

Canonical copies live in [`skills/`](skills/) — install them into each agent's
skill directory (below).

`new-git-project` is **additive and idempotent**: it creates only what's missing
and only commits a fresh repo — so you can run it right after `new-project` to
*add git* to an already-scaffolded project, or re-run it harmlessly.

## Why Skills, and why two of them

- **Cross-agent, one spec.** A single `SKILL.md` (open [Agent Skills](https://agentskills.io)
  format: `name` + `description` + body) is understood by both tools. This
  replaces the earlier Claude-only `/new-project` *command* — a command and a
  skill doing the same job would drift.
- **Deterministic git behavior.** The git split is deliberate. When git wasn't
  specified, the two agents diverged (Codex ran `git init`, Claude didn't). Now
  the choice is explicit: pick the skill for the behavior you want.
- **`model: haiku`** — scaffolding is grunt work. Claude Code honors this field;
  Codex ignores it (model is session-level there) — so the one shared file keeps
  the optimization on the Claude side without breaking Codex.

## Install — copy to BOTH agent skill dirs

Claude Code and Codex read **different, non-overlapping** skill directories, so
each skill folder must be placed in both (the open standard fixes the format,
not the location):

- **Claude Code:** `~/.claude/skills/<name>/SKILL.md`
- **Codex:** `~/.agents/skills/<name>/SKILL.md`
  (Windows: `C:\Users\<you>\.agents\skills\`; `~/.codex/skills/` is reserved for
  Codex's system skills — don't author there.)

Copy both `skills/new-project/` and `skills/new-git-project/` into each location.
The repo copy here is the source of truth; re-copy after edits.

## Usage

- **Claude Code:** `/new-project` or `/new-git-project` (or let Claude
  auto-trigger from the description).
- **Codex:** `$new-project` / `$new-git-project`, or pick from `/skills`.

There's no argument-placeholder syntax on the Codex side, so state the project
name/description in your message; the skill asks if it's missing.

## Gotchas

- **No single shared location** — the same skill lives in two dirs; keep them in
  sync from the repo copy.
- **Codex Skills** need Codex CLI v0.65+; confirm via `/skills`. Some v0.117.0
  builds dropped skill discovery — restart Codex if a skill doesn't appear.

## Related

- [Keeping CLAUDE.md and AGENTS.md in Sync](syncing-claude-md-and-agents-md.md)
  — the pattern these skills set up (and its global-config variant).
- [Leverage Subagents and Skills with Reduced Models](../agents/claude/reduced-models-for-subagents-and-skills.md)
  — why `model: haiku` fits scaffolding.

## Sources

- https://agentskills.io/specification
- https://learn.chatgpt.com/docs/build-skills
- https://code.claude.com/docs/en/skills.md
