# Tool Evaluation: Caveman

**Verdict: evaluated, NOT adopted.** Real and safe, but a novelty — the
headline savings are oversold for actual coding work.

- Repo: https://github.com/JuliusBrussee/caveman (MIT, ~90k stars, actively
  maintained as of July 2026)
- What it is: a Claude Code **skill** (a prompt, not engineering) that tells the
  model to strip pleasantries, articles, and filler while preserving code,
  error strings, and git/PR text. Commands: `/caveman [lite/full/ultra]`,
  `/caveman-commit`, `/caveman-review`, `/caveman-stats`.

## Why not adopted

| Claim | Reality |
| --- | --- |
| "65% fewer output tokens" | From the repo's own benchmark of **10 chat-style prose prompts** — the best case for filler-stripping. |
| "Same answers" | Fair — independent testing found quality is neutral (no meaningful degradation). |
| Real-world savings | **~8.5%** on actual coding tasks (JetBrains independent benchmark, ~240 trials on 87 SkillsBench tasks, Sonnet 5). Agentic output is mostly code/diffs/tool-calls/error-strings, which Caveman doesn't touch. |
| Net cost impact | Only shrinks **output** tokens, and the skill **adds ~1–1.5k input tokens per turn** — net savings on real work can approach zero on verbose tasks. |

JetBrains' summary: *"Safe, honest about style, oversold on savings."*

Additional cost: terser "caveman" output reduces readability of explanations in
PR/review contexts — a real cost against a tiny token gain.

## Security notes (if ever revisited)

- Install is `curl -fsSL .../install.sh | bash` (or `irm .../install.ps1 | iex`).
- The wrapper delegates to `bin/install.js`, which is what actually writes skill
  files into agent config dirs. **Audit `bin/install.js` before running** — the
  install has no checksum/signature verification.
- Open source, MIT, claims zero telemetry — auditable.

## Better levers for the same goal

- [Be Concise With Responses](be-concise-responses.md) — captures most of the
  benefit with no dependency.
- Prompt caching and scoping context are far larger levers if token cost is a
  genuine concern.

## Sources

- https://github.com/JuliusBrussee/caveman
- https://blog.jetbrains.com/ai/2026/07/speak-to-ai-agents-like-cavemen-tosave-tokens/
- https://thenewstack.io/caveman-mode-token-savings/
- https://www.claudepluginhub.com/plugins/juliusbrussee-caveman
