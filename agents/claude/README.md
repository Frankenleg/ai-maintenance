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

## Tool evaluations

- [Caveman](caveman-plugin-evaluation.md) — **evaluated, not adopted.** Real and
  safe, but ~8–10% real savings (not the advertised 65%); a novelty.
- [rtk ("rtk AI")](rtk-cli-proxy-evaluation.md) — **evaluated, not adopted.**
  Real, but ~single-digit real savings (not 60–90%) and an open critical
  shell-injection report; not a proxy for LLM traffic.

---

Images for these docs live in [`images/`](images/).
