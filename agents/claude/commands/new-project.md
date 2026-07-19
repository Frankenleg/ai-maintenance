---
description: Scaffold a new project's agent instructions (AGENTS.md canonical + CLAUDE.md import)
argument-hint: [optional one-line project description]
model: haiku
---

Set up this project's agent instruction files using the "AGENTS.md canonical,
CLAUDE.md imports it" pattern, so Claude Code and Codex share one source of
truth. Follow these steps exactly:

1. **Don't clobber.** Check for an existing `AGENTS.md` or `CLAUDE.md` in the
   project root. If either exists, DO NOT overwrite it — show me what's there
   and ask how to proceed.

2. **Create `AGENTS.md`** as a LIGHT, project-specific skeleton. Fill in what
   you can infer from the repo (name, stack, build/test/run commands); leave
   clear `<placeholders>` for the rest. Do NOT copy global rules (git workflow,
   notifications, etc.) — those come from global config. Keep it well under the
   ~200-line target. Use this shape:

   ```markdown
   # <Project Name>

   <One-line description of what this project is.>

   ## Stack & layout
   - <language/framework, key directories, entry points>

   ## Build / test / run
   - Build: <command>
   - Test: <command>
   - Run: <command>

   ## Conventions
   - <project-specific conventions only>

   ## Gotchas
   - <anything non-obvious about this project>
   ```

   If `$ARGUMENTS` is provided, use it as the project's one-line description.

3. **Create `CLAUDE.md`** containing exactly:

   ```markdown
   @AGENTS.md

   <!-- AGENTS.md is the canonical instruction file (shared with Codex). Claude
        Code reads CLAUDE.md, so this file just imports it. Add Claude-only
        instructions below the import if ever needed. -->
   ```

4. Show me both files and a one-line summary. Create nothing else.
