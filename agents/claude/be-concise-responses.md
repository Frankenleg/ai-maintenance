# Be Concise With Responses

A simple instruction to reduce **output** tokens: tell Claude to be concise.

> Note: this targets **output** tokens (what Claude writes back), a different
> lever than trimming session-start **input** context (see
> [Clean Up Session-Start Context](clean-up-session-start-context.md) and
> [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)).

## The instruction

Add to `CLAUDE.md`:

```text
Be concise with all of your responses.
```

## Recommended phrasing (with caveat)

Plain "be concise" can over-correct — for coding work you still want full
explanations when a change is risky. Prefer a phrasing that keeps that safety
valve:

```text
Be concise by default. Skip preamble and restate-the-question filler. Stay
thorough when correctness is at stake — explain risky changes, edge cases, and
anything a reviewer needs to catch a mistake.
```

## Why not go further (e.g. "caveman speak")

Gimmicks that strip grammar to shave tokens trade away the clarity that keeps
bad changes from merging (commit messages, review feedback, explanations of
risky edits). Output tokens are also usually the cheaper direction to optimize
— the larger wins are in trimming what pre-loads every session. A measured
conciseness directive captures most of the benefit without the correctness risk.

> **Cross-project note:** "be concise" is a universal preference and a good
> candidate for global `~/.claude/CLAUDE.md`. Kept as a documented tip for now.

## Related

- [Clean Up Session-Start Context](clean-up-session-start-context.md) — the
  input-side counterpart to this output-side lever.
- [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)
- [Tool Evaluation: Caveman](caveman-plugin-evaluation.md) — the gimmick version
  of this idea, and why a plain instruction beats it.
