---
name: new-git-project
description: Scaffold a new project AS A GIT REPOSITORY — git init, a minimal .gitignore and .gitattributes, a light canonical AGENTS.md plus a CLAUDE.md that imports it, and an initial commit on main. Use when starting a new project/repo that should be version-controlled, or to add git to a project already scaffolded by new-project. Does NOT create a remote. Use new-project instead if you do NOT want git.
model: haiku
---

# New Git Project Scaffolder

Set up a new project as a git repository with agent instruction files, using the
"AGENTS.md canonical, CLAUDE.md imports it" pattern. Every step is **additive and
idempotent** — safe to run on an empty directory, or right after `new-project`
(it just adds git), or again on an already-set-up project.

## Steps

1. **Determine the project name and a one-line description.**
   - If the user provided them (in the conversation or as arguments), use those.
   - Otherwise, if you can ask the user, ask before writing anything.
   - If you cannot ask (running autonomously / auto-invoked), do **not** invent a
     name like "Test". Default the **name** to the current directory's name,
     leave the **description** as a literal placeholder
     (`<one-line description — fill in>`), and in your final report state both
     assumptions clearly so the user can correct them.

2. **Ensure git is initialized.** If the directory is **not** already a git repo,
   run `git init` and ensure the default branch is `main`. If it already is a
   repo, leave it as-is.

3. **Create a minimal `.gitignore` only if one doesn't already exist** (keep any
   existing one):

   ```gitignore
   # OS / editor cruft
   Thumbs.db
   .DS_Store
   *.tmp

   # Secrets — never commit
   .env
   .env.*
   *.pem
   *.key
   id_rsa
   ```

4. **Create a `.gitattributes` only if one doesn't already exist** (keep any
   existing one). This normalizes line endings — no "LF will be replaced by
   CRLF" warnings on Windows, and consistent endings across platforms:

   ```gitattributes
   # Normalize line endings: store LF in the repo, check out native per-OS.
   * text=auto

   # Shell scripts must stay LF so they run on all platforms.
   *.sh text eol=lf
   ```

5. **Create the instruction files that are missing.** For `AGENTS.md` and
   `CLAUDE.md`: **keep any that already exist** (do not overwrite — running this
   right after `new-project` is expected), and create only the missing one(s).

   `AGENTS.md` is a LIGHT, project-specific skeleton — infer what you can (name,
   stack, build/test/run commands), leave clear `<placeholders>` for the rest,
   don't copy global rules, keep it under the ~200-line target:

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

   `CLAUDE.md` is exactly:

   ```markdown
   @AGENTS.md

   <!-- AGENTS.md is the canonical instruction file. Codex reads AGENTS.md
        natively; Claude Code reads CLAUDE.md, so this file imports it. Add
        Claude-only instructions below the import if ever needed. -->
   ```

6. **Make the initial commit only if the repo has no commits yet.** If this is a
   fresh repo (no `HEAD`/commits), stage the files and commit as
   `Initial project scaffold` on `main` — this baseline commit is expected. If
   the repo **already has commit history**, do NOT add a commit; leave staging to
   the user's normal workflow.

7. **Do NOT create a remote.** Creating/pushing to a remote is a separate,
   deliberate step for the user.

8. **Report** what you did — git initialized or already present, which files were
   created vs. found existing, and whether an initial commit was made.
