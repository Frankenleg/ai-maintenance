# Claude

Docs for maintaining Claude / Claude Code.

## Topics

### Context & token management

- [Clean Up Session-Start Context](clean-up-session-start-context.md) — audit
  MCPs, Skills, and CLAUDE.md that pre-load at session start, and trim them.
- [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md) —
  relocate workflow-specific instructions into on-demand skills to shrink base
  context.
- [Offload Processing to Hooks and Skills](offload-processing-to-hooks-and-skills.md)
  — preprocess data in code (e.g. grep a log) and feed Claude only the distilled
  result.
- [Be Concise With Responses](be-concise-responses.md) — reduce output tokens
  with a measured conciseness instruction (thorough when correctness matters).
- [Leverage Subagents and Skills with Reduced Models](reduced-models-for-subagents-and-skills.md)
  — assign `model: haiku` to grunt work and `context: fork` to self-contained
  skills; keep the frontier model for judgment.
- [Move Workflows into Script-Driven Skills](script-driven-skills.md) — bundle
  scripts in skills and run deterministic steps as code instead of token-by-token.

## Tool evaluations

- [Caveman](caveman-plugin-evaluation.md) — **evaluated, not adopted.** Real and
  safe, but ~8–10% real savings (not the advertised 65%); a novelty.
- [rtk ("rtk AI")](rtk-cli-proxy-evaluation.md) — **evaluated, not adopted.**
  Real, but ~single-digit real savings (not 60–90%) and an open critical
  shell-injection report; not a proxy for LLM traffic.
- [Superpowers](superpowers-plugin-evaluation.md) — **recommended (situational),
  in active use.** Reputable, safe, MIT; workflow discipline worth it for
  substantial work, bypassable for trivial edits.

---

Images for these docs live in [`images/`](images/).
