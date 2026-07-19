# Move Workflows into Script-Driven Skills

A skill is a **folder**, not just a `SKILL.md` — it can bundle scripts
(Python, bash, etc.), and `SKILL.md` can instruct the model to **run** them
instead of performing deterministic steps itself, token by token.

Moving deterministic steps into scripts makes them **cheaper, faster, and more
reliable** — deterministic code doesn't hallucinate a step or drift between
runs. This is the [offload-to-code principle](offload-processing-to-hooks-and-skills.md)
applied *inside* a skill, and it matches Anthropic's own skill-authoring
guidance.

## The right shape

`SKILL.md` **orchestrates**; scripts do the **deterministic** work.

- **Script it:** parsing, formatting, file transforms, API calls, validation,
  data extraction, repetitive multi-step procedures — anything mechanical and
  repeatable.
- **Keep it as model instructions:** judgment, synthesis, natural-language
  decisions, anything that needs the model's flexibility.

> **Caveat — don't over-script.** Hardcoding a step that actually needs the
> model's flexibility trades reliability for rigidity and makes the skill
> brittle. Only migrate steps that are genuinely deterministic.

## Migration audit prompt

Get recommendations without changing anything (source: @austin.marchese;
lightly cleaned up):

```text
Analyze my project and identify steps in my existing skills that can be
programmatic scripts. Share those skills as well as how you'd migrate them to
script-optimized skills.
```

Review the proposed migrations, then apply the ones that are truly
deterministic.

## Bonus: discover new skills from history

A separate but related maintenance technique bundled into the same prompt —
skill *discovery* rather than skill *optimization*:

```text
Review my entire conversation history to identify any additional skills I
should create.
```

Useful for spotting repeated workflows that deserve to become a skill in the
first place.

## Related

- [Offload Processing to Hooks and Skills](offload-processing-to-hooks-and-skills.md)
  — same principle, at the hook/tool-output level.
- [Leverage Subagents and Skills with Reduced Models](reduced-models-for-subagents-and-skills.md)
  — the sibling efficiency lever (model selection + context forking).
- [Move Instructions from CLAUDE.md to Skills](move-instructions-to-skills.md)
  — get workflow instructions into skills in the first place.
