# Move Instructions from CLAUDE.md to Skills

A technique for keeping session-start context small: relocate specialized,
workflow-specific instructions out of `CLAUDE.md` and into **Skills**.

## Why

`CLAUDE.md` is loaded into context at **session start**. If it contains
detailed instructions for specific workflows (e.g. PR reviews or database
migrations), those tokens are present **even when you're doing unrelated
work** — pure overhead most of the time.

A skill's **description** loads at session start (so Claude knows the skill
exists and when to invoke it); its **full instructions/body** load on-demand
only when invoked. Moving specialized instructions into a skill therefore keeps
your base context smaller — the bulky body is deferred.

> **Caveat:** it isn't free. Each skill's description *does* stay in base
> context at startup, so keep descriptions tight — only the body is deferred.

## Rule of thumb

Aim to keep `CLAUDE.md` **under 200 lines** by including only essentials.
Everything that's a specific workflow rather than an always-relevant essential
is a candidate to move into a skill.

## How to decide what moves

| Keep in CLAUDE.md                          | Move to a Skill                          |
| ------------------------------------------ | ---------------------------------------- |
| Always-relevant essentials & conventions   | Specific workflows (PR review, migrations) |
| Pointers to how the system is organized     | Step-by-step procedures used occasionally |
| Rules that apply to (nearly) every session  | Instructions only relevant when a task starts |

## Related

- [Clean Up Session-Start Context](clean-up-session-start-context.md) — the
  audit routine that surfaces what's bloating CLAUDE.md, Skills, and MCPs.

> **Cross-project note:** "keep CLAUDE.md concise / under ~200 lines" is a
> universal practice (candidate for global `~/.claude/CLAUDE.md`), currently
> kept as a documented tip only.
