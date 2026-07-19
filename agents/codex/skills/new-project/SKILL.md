---
name: new-project
description: Scaffold a new project's agent instruction files — a light canonical AGENTS.md plus a CLAUDE.md that imports it. Use when starting a new or empty project/repo that has no AGENTS.md or CLAUDE.md yet. Do not use to edit an existing, already-scaffolded instruction set.
---

# New Project Scaffolder

Set up a new project's agent instruction files using the "AGENTS.md canonical,
CLAUDE.md imports it" pattern, so Codex and Claude Code share one source of
truth.

## Steps

1. **Determine the project name and a one-line description.** If the user hasn't
   provided them in the conversation, ask before writing anything.

2. **Don't clobber.** If `AGENTS.md` or `CLAUDE.md` already exists in the project
   root, do NOT overwrite it — show what's there and ask how to proceed.

3. **Create `AGENTS.md`** as a LIGHT, project-specific skeleton. Fill in what you
   can infer from the repo (name, stack, build/test/run commands); leave clear
   `<placeholders>` for the rest. Do NOT copy global rules (git workflow, etc.) —
   those come from global config. Keep it well under the ~200-line target. Shape:

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

4. **Create `CLAUDE.md`** containing exactly:

   ```markdown
   @AGENTS.md

   <!-- AGENTS.md is the canonical instruction file. Codex reads AGENTS.md
        natively; Claude Code reads CLAUDE.md, so this file imports it. Add
        Claude-only instructions below the import if ever needed. -->
   ```

5. **Report** the files created. Create nothing else.
