---
name: new-project
description: Scaffold a new project's agent instruction files — a light canonical AGENTS.md plus a CLAUDE.md that imports it. Use when starting a new or empty project/repo that needs baseline agent guidance and you do NOT want a git repository created. Use new-git-project instead if you also want git initialized. Do not use to edit an existing, already-scaffolded instruction set.
model: haiku
---

# New Project Scaffolder (no git)

Set up a new project's agent instruction files using the "AGENTS.md canonical,
CLAUDE.md imports it" pattern, so Codex and Claude Code share one source of
truth. **Do not initialize git** — this creates instruction files only. (Use the
`new-git-project` skill if you also want a git repository.)

## Steps

1. **Determine the project name and a one-line description.**
   - If the user provided them (in the conversation or as arguments), use those.
   - Otherwise, if you can ask the user, ask before writing anything.
   - If you cannot ask (running autonomously / auto-invoked), do **not** invent a
     name like "Test". Default the **name** to the current directory's name,
     leave the **description** as a literal placeholder
     (`<one-line description — fill in>`), and in your final report state both
     assumptions clearly so the user can correct them.

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

5. **Do not run any git commands.**

6. **Report** the files created. Create nothing else.
