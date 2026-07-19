# AI Maintenance — Agent Instructions

Documentation repo for managing and maintaining AI coding agents. These
instructions apply to any agent doing work in this repo. Shared with Claude
Code (via `CLAUDE.md`'s `@AGENTS.md` import) and Codex.

## Repository layout

- `agents/<name>/` — docs specific to one agent (e.g. `agents/claude/`).
- `shared/` — cross-cutting docs that apply to any agent.
- Each folder has a `README.md` index; images live in its `images/` subfolder.
- See `README.md` for the full map.

## Conventions when adding docs

- **One doc per topic.** Route each item to its proper folder/section; create a
  new topic doc rather than forcing content into an unrelated one.
- **Decompose multi-item sources.** If a source (e.g. a screenshot) contains
  several commands/tips, split them and file each in its proper place.
- **Verify before documenting.** Don't transcribe claims uncritically — check
  them (Claude Code features, tool claims) and push back on anything that's
  inaccurate, outdated, or overstated. Frame heuristics as heuristics.
- **Images sparingly.** Capture information as text; save an image only when it
  genuinely supports the doc.
- **Keep indexes updated.** Update the relevant `README.md` when adding a doc.
- **Tool evaluations** go under `agents/<name>/` with a clear verdict
  (adopted / recommended-situational / not adopted) and sources.

## Conciseness

Keep instruction files (this one, `CLAUDE.md`) and the docs concise — a
directory, not documentation.
