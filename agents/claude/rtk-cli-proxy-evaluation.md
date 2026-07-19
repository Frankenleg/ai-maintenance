# Tool Evaluation: rtk ("rtk AI")

**Verdict: evaluated, NOT adopted.** Real and open-source, but the "proxy"
label is misleading, the headline savings are inflated, and there's an open
critical-severity security report.

- Repo: https://github.com/rtk-ai/rtk (Rust, Apache-2.0, v0.43.0, active). Real
  homebrew tap with valid checksums. Badges check out. ~72k stars in under 6
  months — anomalous velocity; treat as a hype signal, not a quality signal.

## What it actually is (important correction)

It is **NOT a proxy for your LLM traffic.** It never sees your prompts,
responses, or Anthropic API key — the obvious "proxy sees my keys" fear does
**not** apply.

What it really does: installs a Claude Code **PreToolUse hook** that rewrites
shell commands (`git status` → `rtk git status`), runs them **locally**, and
**compresses the stdout/stderr** (noise filtering, dedupe repeated log lines,
truncation) before that text enters context.

> This is a productized version of
> [Offload Processing to Hooks and Skills](offload-processing-to-hooks-and-skills.md).
> Much of the benefit is achievable for free by having Claude `grep`/filter
> command output instead of reading it wholesale.

## Why not adopted

| Claim | Reality |
| --- | --- |
| "Reduces token consumption 60–90%" | That's the **% of raw CLI output stripped**, not a bill cut. Ignores the real cost drivers (file reads, system prompt, history). |
| Real-world savings | **Single-digit percent.** One independent HN run: ~$4.96 saved on a $926 bill (~0.5%). |
| "Same quality" | Unverified. **Silent-degradation risk:** if it truncates a stack trace/compiler error, the agent debugs blind without knowing context was cut. No independent quality benchmark exists. |

## Security notes

The real risk is **not** traffic interception — it's that rtk **executes shell
commands an LLM rewrote**:

- **Open Critical shell-injection report** (issue #640, filed 2026-03-16, still
  open) flags an `sh -c` surface — the exact class that's dangerous when
  LLM-crafted commands flow through it.
- Keeps a local **SQLite command-history DB** (~90-day retention) that could
  persist secrets from command output.
- **Telemetry** claimed opt-in/off-by-default, but a security reviewer disputes
  this. The "Security Check passing" badge is a **self-authored static lint,
  not an independent audit**.
- **Install gotcha:** `cargo install rtk` may pull an unrelated typosquat
  package. Use the official homebrew tap only.

## If ever revisited

- Install via the official homebrew tap (`brew install rtk-ai/tap/rtk`), not
  `cargo install`.
- Leave telemetry off; audit `install.sh`; try on a non-sensitive project first.
- Re-check whether issue #640 (critical shell-injection) has been resolved.

## Better lever for the same goal

- [Offload Processing to Hooks and Skills](offload-processing-to-hooks-and-skills.md)
  — the same principle, DIY, no third-party shell-execution layer.

## Sources

- https://github.com/rtk-ai/rtk
- https://github.com/rtk-ai/rtk/issues/640 (open security review)
- https://mroczek.dev/articles/the-token-compression-illusion-why-im-skeptical-of-rtk/
- https://news.ycombinator.com/item?id=48588755
