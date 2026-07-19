# Session Cleanup & Retention

How Claude Code's saved sessions (the desktop sidebar / `--resume` list) are
retained and pruned — and what you can safely ignore vs. act on.

Sessions are stored as transcript files at
`~/.claude/projects/<encoded-project-path>/<session-id>.jsonl` (the sidebar list
mirrors these files one-to-one).

## Auto-cleanup (the default is fine)

Claude Code deletes old session transcripts **at startup**, controlled by
`cleanupPeriodDays` — **default 30 days**. The sidebar self-prunes; you don't
need to manage it manually.

Tune it in `~/.claude/settings.json` only if you want to:

```json
{ "cleanupPeriodDays": 14 }
```

- **Lower** (7–14) for more privacy / less clutter with sensitive output.
- **Raise** (60+) if you frequently resume old conversations or use `/rewind`.

## What survives vs. what's lost — the part people miss

Cleanup (or manual deletion) removes **only the transcript**, not your config:

| Preserved | Lost (for deleted sessions only) |
| --- | --- |
| Memory files (`.../memory/*.md`) — **not** deleted by `cleanupPeriodDays` | `--resume` / `--continue` for that session |
| `CLAUDE.md` / `AGENTS.md`, `settings.json`, permissions | `/rewind` checkpoints for that session |

So letting sessions age out costs you nothing structural — memory and project
config stay intact.

## Bulk cleanup (CLI — no desktop multi-select)

The sidebar has no "clear all" button; use the CLI (Claude Code v2.1.124+):

```bash
claude project purge            # current project — shows a plan, asks to confirm
claude project purge --all      # every project at once
claude project purge ~/path --yes   # scripted, no prompt
```

Deleting the `.jsonl` files by hand is also safe.

## Keeping a session past the window

To preserve a specific conversation beyond the retention period (a decision log,
a tricky debug), **`/export`** it to a file before it ages out. Everything else,
let auto-cleanup handle.

## Recommendation

Leave the **30-day default**. Start new sessions freely when swapping tasks —
they stay resumable for the window, and nothing you care about is lost when they
drop off. Reach for `/export` only for the rare session worth keeping forever.

## Sources

- https://code.claude.com/docs/en/sessions.md
- https://code.claude.com/docs/en/claude-directory.md
- https://code.claude.com/docs/en/settings.md (`cleanupPeriodDays`)

## Related

- [Clean Up Session-Start Context](clean-up-session-start-context.md) — trimming
  what *loads into* a session (a different lever than pruning old sessions).
