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

## Global (user-level) instructions across agents

The setup above is for **per-project** files. Global/user-level sync is a
separate problem because each tool keeps its global file in its own home and
**Codex's global file has no import mechanism**:

- **Claude Code global:** `~/.claude/CLAUDE.md` — supports `@import`
  (absolute paths).
- **Codex global:** `C:\Users\<you>\.codex\AGENTS.md` — read natively and
  merged root-to-leaf, but **no import/include** (open issues #6038, #17401).
  Blogs claiming `@file` imports work in Codex are wrong (that's Claude
  Code / Gemini).

### Recommended: `~/.codex/AGENTS.md` is canonical; Claude imports it

Codex requires its global instructions at `~/.codex/AGENTS.md` and can't
import, so make **that file the single source of truth**. In
`~/.claude/CLAUDE.md`:

```markdown
@C:\Users\<you>\.codex\AGENTS.md

## Claude-only global instructions
(anything that should apply to Claude Code but not Codex)
```

- **No symlink/hardlink, no admin, no copy step** — one real file, edited once,
  both agents read it. (A hardlink to a canonical file elsewhere also works
  — `mklink /H`, same NTFS volume, no admin — but editors that save via
  atomic-replace can sever the link, so the direct import is cleaner.)
- Avoid Codex's `config.toml` `model_instructions_file` for this — its
  semantics are ambiguous (it may *replace* Codex's base prompt).

### Handling per-agent differences (e.g. branch prefix)

Both agents read the same bytes, so write shared instructions **generically and
let each agent self-select** rather than branching the file:

```text
Branch off main into a branch prefixed with your agent name — claude/<short-name>
if you are Claude Code, codex/<short-name> if you are Codex. Never commit
directly to main.
```

Genuinely agent-only lines go in each tool's native wrapper (`CLAUDE.md` below
its import); keep the shared body generic.

### Scope

- Syncs **instructions**, not **settings**. Codex settings (model, approval,
  sandbox, MCP) live in `~/.codex/config.toml` — the analog of Claude's
  `settings.json`; different format, not syncable this way.

## Scope notes

- Since `AGENTS.md` (project or global) now feeds both tools, the "keep it
  concise / a directory, not documentation" hygiene applies to it too
  (see [Clean Up Session-Start Context](../agents/claude/clean-up-session-start-context.md)
  and [Move Instructions from CLAUDE.md to Skills](../agents/claude/move-instructions-to-skills.md)).

## Related

- [Orchestrating Claude Code and Codex](orchestrating-claude-and-codex.md)
- [Scaffolding New Projects](scaffolding-new-projects.md) — the `new-project` /
  `new-git-project` skills scaffold this exact `AGENTS.md` + `CLAUDE.md` pair.

## Sources

- https://code.claude.com/docs/en/memory.md
