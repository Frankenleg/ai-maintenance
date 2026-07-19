# AI Maintenance — Agent Instructions

Documentation repo for managing and maintaining AI coding agents. These
instructions apply to any agent doing work in this repo. Shared with Claude
Code (via `CLAUDE.md`'s `@AGENTS.md` import) and Codex.

## Repository layout

- `agents/<name>/` — docs specific to one agent (e.g. `agents/claude/`).
- `shared/` — cross-cutting docs that apply to any agent.
- Each folder has a `README.md` index; if a doc needs images, add them in an
  `images/` subfolder beside it.
- See `README.md` for the full map.

## Conventions when adding docs

- **One doc per topic.** Route each item to its proper folder/section; create a
  new topic doc rather than forcing content into an unrelated one.
- **Decompose multi-item sources.** If a source (e.g. a screenshot) contains
  several commands/tips, split them and file each in its proper place.
- **Verify before documenting.** Don't transcribe claims uncritically — check
  them (Claude Code features, tool claims) and push back on anything that's
  inaccurate, outdated, or overstated. Frame heuristics as heuristics.
- **Link to official docs; don't replicate them.** A copy of upstream docs rots
  silently and adds bulk without value. Capture only the *delta* — the verified
  fact you rely on plus what the docs don't give you (a caveat, a decision, the
  "what actually happens", a recommendation) — and cite/link the source. If a
  doc would just restate official docs with nothing added, drop a link instead.
- **Images sparingly.** Capture information as text; save an image only when it
  genuinely supports the doc.
- **Keep indexes updated.** Update the relevant `README.md` when adding a doc.
- **Copy-paste items go in fenced code blocks** — anything meant to be pasted
  verbatim (prompts, rules for CLAUDE.md/AGENTS.md, commands, snippets) uses a
  code block, not a blockquote. Reserve blockquotes for quotes, notes, and
  caveats.
- **Keep it relational.** Every topic doc has a `## Related` section linking its
  siblings; every doc is reachable from a `README.md` index; use relative links
  so navigation works from the repo or GitHub.
- **Tool evaluations** go under `agents/<name>/` with a clear verdict
  (adopted / recommended-situational / not adopted) and sources.

## Conciseness

Keep the instruction set concise — a directory, not documentation. Because
`CLAUDE.md` just imports this file, `AGENTS.md` is the content that matters;
measure its length here. Aim for **under ~200 lines as a target, not a hard
requirement** — a smell threshold for "trim this," not a gate.
