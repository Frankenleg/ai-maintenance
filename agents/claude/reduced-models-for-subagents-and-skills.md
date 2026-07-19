# Leverage Subagents and Skills with Reduced Models

Match the model to the work. Run grunt work on a cheap model (Haiku) and
reserve the frontier model for judgment, and isolate ("fork") context when a
task doesn't need the conversation. Real cost savings applied only where
quality doesn't suffer — no third-party tool, no attack surface.

> All frontmatter keys below are verified against the official Claude Code docs
> ([subagents](https://code.claude.com/docs/en/sub-agents.md),
> [skills](https://code.claude.com/docs/en/skills.md)).

## The two levers

| Lever | What it does | Where it applies |
| --- | --- | --- |
| `model:` | Run on a cheaper/stronger model (`haiku`, `sonnet`, `opus`, `fable`, a full model ID, or `inherit`). | **Both** subagents and skills. |
| `context: fork` | Run in an isolated subagent context that does **not** see the main conversation. Pair with `agent:` to pick the subagent type. | **Skills only** — subagents are always context-isolated by design. |

## Grunt work vs. judgment work

- **Grunt work → `model: haiku`.** Scrape, summarize, classify, fetch, format —
  mechanical tasks where a smaller model is fine.
- **Judgment work → keep the top model.** Synthesize, decide, judge — reasoning
  where frontier quality matters.

## Decision table (skill configuration)

A skill runs inline in the main conversation by default, so both levers apply.
Decide per skill:

| Needs conversation context? | Needs frontier brain? | Frontmatter |
| --- | --- | --- |
| No  | No  | `context: fork` + `model: haiku` — full win |
| Yes | No  | `model: haiku` only — cheap, keeps the thread |
| No  | Yes | `context: fork` only — hygiene, keeps quality |
| Yes | Yes | neither — that's main-thread work |

For **subagents**, context is always isolated, so `model:` is the only lever.

### Example frontmatter

```yaml
# .claude/skills/summarize/SKILL.md — self-contained grunt work
---
name: summarize
description: Summarize fetched content
context: fork
agent: Explore
model: haiku
---
```

```yaml
# .claude/agents/research.md — grunt-work subagent (already isolated)
---
name: research
description: Gather and format raw data
model: haiku
---
```

## Audit prompt: Find the Minimum Viable Model

Run this to get recommendations without changing anything (source:
@austin.marchese):

> Find the Minimum Viable Model for every skill in `.claude/skills/` and
> subagent in `.claude/agents/`.
>
> - Grunt work (scrape, summarize, classify, fetch, format) → `model: haiku`.
> - Judgment work (synthesize, decide, judge) → keep the top model.
>
> Return a table: file → recommended change. Don't change anything yet.

The "return a table, don't change anything yet" pattern (same as the
[session-start cleanup](clean-up-session-start-context.md) routine) keeps the
audit review-then-approve rather than acting unprompted.

## Related

- [Offload Processing to Hooks and Skills](offload-processing-to-hooks-and-skills.md)
  — the sibling efficiency lever: preprocess in code so the model sees less.
- [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)
