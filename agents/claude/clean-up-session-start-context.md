# Clean Up Session-Start Context

A periodic maintenance routine: audit everything that pre-loads into Claude's
context at the **start of a session** and trim it down. Less start-of-session
context means more room for actual work and less noise for the model.

Source: prompt shared by @austin.marchese.

## The prompt

Paste this to Claude to run the full audit:

> Audit everything that pre-loads into my context at the start of a session and
> help me cut it down. Three parts:
>
> 1. **MCPs:** list every connected MCP server and flag the ones I haven't
>    used. Recommend which to remove.
> 2. **Skills:** list my skills and their descriptions. Flag unused ones to
>    delete, and shorten any description longer than it needs to be.
> 3. **CLAUDE.md:** review it as a directory, not documentation. Trim it toward
>    under 200 lines without losing how the system works. Show me the list with
>    your recommendations before removing or editing anything.

## What each part does

### 1. MCP servers
List every connected MCP server, flag ones that go unused, and recommend which
to remove. Fewer connected servers = fewer tool schemas loaded up front.

### 2. Skills
List all skills with their descriptions. Flag unused skills for deletion, and
shorten any description that's longer than it needs to be (descriptions are
what the model reads to decide when a skill applies — they should be tight).

### 3. CLAUDE.md
Review CLAUDE.md **as a directory, not documentation** — it should point to how
the system works, not re-explain it. Trim toward **under 200 lines** without
losing how the system works.

> **Cross-project note:** keeping CLAUDE.md concise (a directory, not
> documentation; aim under ~200 lines) is a universal practice, not just an
> AI-maintenance reference. See global-config wiring in your `~/.claude/CLAUDE.md`.

## Key safety rule

Always have Claude **show the list with its recommendations *before* removing or
editing anything.** Review, then approve — don't let the cleanup delete or
rewrite unprompted.
