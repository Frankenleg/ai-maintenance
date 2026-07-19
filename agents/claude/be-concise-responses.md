# Be Concise With Responses

An instruction to reduce **output** tokens — concise by default, without
dropping the detail that matters for correctness.

> Note: this targets **output** tokens (what Claude writes back), a different
> lever than trimming session-start **input** context (see
> [Clean Up Session-Start Context](clean-up-session-start-context.md) and
> [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)).

## The instruction

Add this to your instruction file — `CLAUDE.md`, or global config to apply it
everywhere:

```text
Be concise by default. Skip preamble and restate-the-question filler. Stay
thorough when correctness is at stake — explain risky changes, edge cases, and
anything a reviewer needs to catch a mistake.
```

## Why not just "be concise"?

A bare `Be concise with all of your responses` can over-correct — for coding
work you still want full explanations when a change is risky. The phrasing above
keeps that safety valve, so use it instead of the blunt version.

## Why not go further (e.g. "caveman speak")

Gimmicks that strip grammar to shave tokens trade away the clarity that keeps
bad changes from merging (commit messages, review feedback, explanations of
risky edits). Output tokens are also usually the cheaper direction to optimize
— the larger wins are in trimming what pre-loads every session. A measured
conciseness directive captures most of the benefit without the correctness risk.

> **Applied globally:** this lives in `~/.claude/CLAUDE.md` (under "Response
> style"), so it applies to every project. When the global Claude↔Codex sync is
> set up, move it into the shared canonical file so Codex uses it too — see
> [Keeping CLAUDE.md and AGENTS.md in Sync](../../shared/syncing-claude-md-and-agents-md.md).

## Related

- [Clean Up Session-Start Context](clean-up-session-start-context.md) — the
  input-side counterpart to this output-side lever.
- [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)
- [Tool Evaluation: Caveman](caveman-plugin-evaluation.md) — the gimmick version
  of this idea, and why a plain instruction beats it.
