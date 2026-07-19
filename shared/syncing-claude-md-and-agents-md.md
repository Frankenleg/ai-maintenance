# Keeping CLAUDE.md and AGENTS.md in Sync

If you run both **Claude Code** and **Codex** (or Cursor, Windsurf, etc.) on the
same repos, keep their instruction files in sync so both agents follow the same
conventions. Use **one source of truth**, not two hand-maintained copies.

> Cross-cutting: applies to any repo used by multiple agents.

## The key fact

- **Claude Code reads `CLAUDE.md`, not `AGENTS.md`** (verified against current
  docs; native `AGENTS.md` support is the most-requested unmet feature, issue
  #6235 — not available today).
- **Codex reads `AGENTS.md`** (as do Cursor, Windsurf, Cline).
- **`CLAUDE.md` supports imports** via `@path` syntax — including `@AGENTS.md`.

So you can't drop to a single file, but you can make one file *canonical* and
have the other import it.

## Recommended setup (Windows-friendly, zero maintenance)

Make **`AGENTS.md` canonical** and make `CLAUDE.md` a thin importer:

```
your-repo/
├── AGENTS.md     ← the real content; Codex reads this directly
└── CLAUDE.md     ← imports AGENTS.md (+ any Claude-only overrides)
```

`CLAUDE.md`:

```markdown
@AGENTS.md

## Claude Code-specific (optional)
Anything that should apply ONLY to Claude Code goes here.
```

### Why this approach

- **Officially recommended** — Anthropic's docs recommend exactly this for repos
  already using `AGENTS.md`.
- **No admin rights** — a symlink (`CLAUDE.md → AGENTS.md`) also works, but on
  **Windows it needs Administrator / Developer Mode**. The `@import` needs none.
- **Zero drift** — edit only `AGENTS.md`; `CLAUDE.md` pulls it fresh each
  session.
- **`/init` cooperates** — Claude Code's `/init`, run in a repo that already has
  `AGENTS.md`, auto-generates a `CLAUDE.md` that imports it (it also reads
  `.cursorrules`, `.windsurfrules`, etc.).

### Alternatives (and why not)

| Approach | Verdict |
| --- | --- |
| `@AGENTS.md` import in CLAUDE.md | ✅ Recommended — supported, Windows-safe, no drift. |
| Symlink `CLAUDE.md → AGENTS.md` | ⚠️ Works, but needs Admin/Developer Mode on Windows. |
| Rely on native `AGENTS.md` reading | ❌ Claude Code doesn't read it natively. |
| Manual / scripted two-file sync | ❌ Needless complexity; drifts. |

## Gotchas

- The **first** external import in a project triggers a one-time approval dialog
  in Claude Code — approve it once.
- Import paths resolve **relative to the `CLAUDE.md` file**, so keep `AGENTS.md`
  beside it.
- Imports go up to **4 hops** deep.

## Scope notes

- This is for **per-project** files. Your **global** `~/.claude/CLAUDE.md` stays
  Claude-specific — Codex has its own global config.
- Since `AGENTS.md` now feeds both tools, the "keep it concise / a directory,
  not documentation" hygiene applies to it too
  (see [Clean Up Session-Start Context](../agents/claude/clean-up-session-start-context.md)
  and [Move Instructions from CLAUDE.md to Skills](../agents/claude/move-instructions-to-skills.md)).

## Related

- [Orchestrating Claude Code and Codex](orchestrating-claude-and-codex.md)

## Sources

- https://code.claude.com/docs/en/memory.md
