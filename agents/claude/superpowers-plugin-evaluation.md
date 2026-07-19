# Tool Evaluation: Superpowers

**Verdict: evaluated — recommended (situational). In active use.** Third-party
but reputable, open source, safe, and genuinely valuable. Great for substantial
engineering work; overkill (and bypassable) for trivial edits.

> The repo owner is a solutions architect / software engineer who actively uses
> Superpowers and values its workflow discipline. This evaluation is of the
> plugin itself, not a recommendation to change that.

- Repo: https://github.com/obra/superpowers — author Jesse Vincent (`obra`),
  **MIT**, open source, ~257k stars (extraordinary for its age), very actively
  maintained.
- **Third-party, not Anthropic** — but accepted into Anthropic's **official
  plugin marketplace** (`superpowers@claude-plugins-official`). "Official
  marketplace listing ≠ built by Anthropic." Commercially backed (Prime
  Radiant, with paid enterprise support).
- **Multi-agent:** primarily a Claude Code plugin, but also ported to Codex,
  Cursor, Antigravity, Copilot CLI, and others.

## What it is

A **skills library** plus **one `SessionStart` hook**. The hook injects the
`using-superpowers` skill — the "you have superpowers / **1% rule** (if there's
even a 1% chance a skill applies, you MUST invoke it) / Red Flags" enforcement
block. Other skill **bodies lazy-load on demand** when invoked.

Skills provided include: `brainstorming`, `writing-plans`, `executing-plans`,
`subagent-driven-development`, `test-driven-development`, `systematic-debugging`,
`requesting-code-review`, `receiving-code-review`, `using-git-worktrees`,
`finishing-a-development-branch`, `verification-before-completion`,
`writing-skills`, `dispatching-parallel-agents`.

## Security assessment — low risk

- **One hook (`SessionStart`)** — reads a local markdown file and prints it into
  context. **No network calls, no file writes, no code execution.**
- **No PreToolUse/Stop hooks** — the "enforcement" is *just a prompt*; it can't
  execute or block anything, and it explicitly states your own instructions
  (CLAUDE.md, direct requests) **override** skills.
- **One minor caveat:** the *optional* visual-brainstorming companion runs a
  **localhost** server whose page loads a logo from `primeradiant.com` — a
  "logo beacon" that reveals you opened the page + the version, **only if you
  use it**. No evidence any code or prompts are transmitted. Skippable.
- Fully open source and auditable (Shell + JS, no binaries).

## Cost — corrected from the common myth

- **Startup cost is modest:** only `using-superpowers` (~1–2k tokens) is
  injected; other skill bodies lazy-load when invoked.
- The scary "55k–134k token overhead / $40 a conversation" figures online refer
  to **stacking many plugins/MCP servers**, *not* Superpowers alone.
- The **real cost is behavioral:** the disciplined workflow (brainstorm → plan →
  TDD → review → verify) deliberately does more, so it uses more tokens and time
  per task. That is the point, not a defect.

## Trade-off

| Fits well | Overkill |
| --- | --- |
| Non-trivial features, multi-step work, teams wanting consistent process | Trivial edits, quick questions, exploratory hacking |

It's **overridable by design**, so bypass it for the small stuff.

## Install (interactive `/plugin`, run it yourself)

```
/plugin install superpowers@claude-plugins-official
# or:
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

Restart the client to activate.

## Related evaluations

- [Caveman](caveman-plugin-evaluation.md) — not adopted (oversold novelty).
- [rtk](rtk-cli-proxy-evaluation.md) — not adopted (oversold + security concern).

## Sources

- https://github.com/obra/superpowers
- https://blog.fsck.com/2025/10/09/superpowers/
- https://claude.com/plugins/superpowers
