# Shared

Cross-cutting docs that apply to any AI agent, not just one.

## Topics

- [Orchestrating Claude Code and Codex](orchestrating-claude-and-codex.md) —
  route token-heavy execution to Codex, keep judgment on Claude; the official
  `codex-plugin-cc` bridge, with data/autonomy caveats.
- [Keeping CLAUDE.md and AGENTS.md in Sync](syncing-claude-md-and-agents-md.md)
  — one source of truth for multi-agent repos: make `AGENTS.md` canonical and
  import it from `CLAUDE.md` with `@AGENTS.md`.
- [Scaffolding New Projects](scaffolding-new-projects.md) — cross-agent
  `new-project` / `new-git-project` Skills (source of truth:
  [`ai-skills`](https://github.com/Frankenleg/ai-skills); installed to both
  `~/.claude/skills/` and `~/.codex/skills/`).

---

If a doc here needs images, add them in an `images/` subfolder beside it.
