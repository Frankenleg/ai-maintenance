# Offload Processing to Hooks and Skills

Do deterministic preprocessing in **code**, and feed Claude only the distilled
result — instead of pouring raw data into context and asking the model to sift
through it.

This is arguably the highest-leverage context-reduction technique: filtering a
10,000-line log down to its ERROR lines can turn **tens of thousands of tokens
into hundreds**.

## The principle

If a step is deterministic (grep, filter, count, extract, transform), a script
can do it far cheaper than the model reading everything. Claude should see the
*answer*, not the *haystack*.

## How it works in Claude Code

### Hooks
Custom **hooks** run shell commands at lifecycle events and can inject
preprocessed context. The reduction only happens when the filtering is done
**up front** and Claude is fed just the result:

- A hook (e.g. on prompt submit / session start) runs a script that greps the
  log for `ERROR` and injects **only** the matching lines as context.
- Equivalent lightweight version: have Claude use `grep`/`rg` **instead of**
  `Read` on the whole file.

> **Precision caveat:** a hook does **not** retroactively shrink a file Claude
> has already `Read` — once a full file is the tool result, it's already in
> context. The win comes from doing the filter *before* the model ingests the
> data (or from Claude grepping rather than reading wholesale), not from a hook
> cleaning up after a large read.

### Skills
A **skill** can encapsulate the same preprocessing as a reusable procedure
(e.g. "extract the failing tests from this output"), loaded on demand. Use a
skill when the preprocessing is a repeatable workflow rather than an automatic,
event-triggered step.

## Related

- [Move Workflows into Script-Driven Skills](script-driven-skills.md) — the same
  offload-to-code idea, applied inside a skill.
- [Leverage Subagents and Skills with Reduced Models](reduced-models-for-subagents-and-skills.md)
- [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)
- [Clean Up Session-Start Context](clean-up-session-start-context.md)
- [Tool Evaluation: rtk](rtk-cli-proxy-evaluation.md) — a product that automates
  this technique (and why the DIY version is usually enough).

> **Cross-agent note:** the underlying principle (offload deterministic work to
> code, feed the model only distilled output) is agent-agnostic. It's framed
> here around Claude Code hooks/skills, but is a candidate to promote into
> [`shared/`](../../shared/) once a second agent is documented.
