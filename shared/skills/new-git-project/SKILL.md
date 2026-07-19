---
name: new-git-project
description: Scaffold a new project AS A GIT REPOSITORY — git init, a minimal .gitignore, a light canonical AGENTS.md plus a CLAUDE.md that imports it, and an initial commit on main. Use when starting a new project/repo that should be version-controlled. Does NOT create a remote. Use new-project instead if you do NOT want git.
model: haiku
---

# New Git Project Scaffolder

Set up a new project as a git repository with agent instruction files, using the
"AGENTS.md canonical, CLAUDE.md imports it" pattern.

## Steps

1. **Determine the project name and a one-line description.** If the user hasn't
   provided them in the conversation, ask before writing anything.

2. **Don't clobber.** If `AGENTS.md` or `CLAUDE.md` already exists in the project
   root, do NOT overwrite it — show what's there and ask how to proceed.

3. **Initialize git** if the directory is not already a git repo: run `git init`
   and ensure the default branch is `main`.

4. **Create a minimal `.gitignore`** (only if one doesn't already exist):

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

5. **Create `AGENTS.md`** as a LIGHT, project-specific skeleton. Fill in what you
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

6. **Create `CLAUDE.md`** containing exactly:

   ```markdown
   @AGENTS.md

   <!-- AGENTS.md is the canonical instruction file. Codex reads AGENTS.md
        natively; Claude Code reads CLAUDE.md, so this file imports it. Add
        Claude-only instructions below the import if ever needed. -->
   ```

7. **Make the initial commit** on `main`: stage the new files and commit with a
   message like `Initial project scaffold`. This baseline commit on `main` is
   expected — the "never commit directly to main" rule is for later feature work.

8. **Do NOT create a remote.** Creating/pushing to a remote is a separate,
   deliberate step for the user.

9. **Report** what was created and committed.
