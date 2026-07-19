# Orchestrating Claude Code and Codex

Run two coding agents together and route each task to the one built for it.
This is the cross-*vendor* version of "match the tool to the work" — keep the
thorough reasoner for judgment, hand token-heavy execution to the surgical one.

> Cross-cutting: involves both Claude Code and Codex (OpenAI's coding agent).

## The characterization (a heuristic, not gospel)

| Agent | Built to be… |
| --- | --- |
| **Claude Code** | Thorough: it re-reads, verifies, and thinks before it acts — and you pay tokens for every bit of that. |
| **Codex** | Surgical: get in, make the edit, get out. |

Treat this as a directional generalization, not an absolute — either agent can
be steered against type. It's a useful default for deciding where work goes.

## Routing strategy

Hand to **Codex** the token-heavy, well-bounded *execution*:

- Bulk file edits
- Well-specced builds (the spec is clear; it's mechanical execution)
- A bug still failing after ~2 tries on the main thread

Keep on **Claude Code** the *judgment and orchestration*: deciding what to do,
synthesizing, reviewing, and driving the overall session.

### Suggested CLAUDE.md rule

Source: @austin.marchese. Add to `CLAUDE.md` (project or global):

```text
Hand any token-heavy execution task to Codex instead of running it here — bulk
file edits, well-specced builds, and bugs still failing after 2 tries. Show me
the diff before writing.
```

The "show me the diff before writing" clause is the safety valve — review
before anything is applied.

## The bridge: `openai/codex-plugin-cc`

An **official OpenAI plugin** that lets you drive Codex from inside Claude Code.

- Repo: https://github.com/openai/codex-plugin-cc (verified under the official
  `openai` org, Apache-2.0, open source and auditable).
- **It shells out to your own locally-installed Codex CLI** — it does *not* call
  the OpenAI API with its own keys and is *not* an MCP server. Same auth, same
  config you already use for `codex`.

### Commands

| Command | Purpose |
| --- | --- |
| `/codex:review` | Read-only standard Codex review of current work. |
| `/codex:adversarial-review` | Read-only "skeptic" review that tries to break confidence in the change. |
| `/codex:rescue` | **Delegates actual work** (investigation/fixes) to a Codex subagent. |
| `/codex:transfer` | Hands off the current session as a resumable Codex thread. |
| `/codex:status` / `/codex:result` / `/codex:cancel` | Manage background Codex jobs. |

### Prerequisites

- Node.js 18.18+
- Codex CLI installed (`npm install -g @openai/codex`) and logged in
  (`codex login`)
- Auth via a ChatGPT subscription **or** an OpenAI API key
- **Cost:** delegated/background jobs consume tokens against your existing
  Codex/ChatGPT usage or API billing — the plugin adds no separate charge.

### Install (standard plugin mechanism, auditable)

```
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/reload-plugins
/codex:setup
```

## Security & data considerations

- **Data flow:** reviewing/delegating sends your repo diffs and working context
  to **OpenAI** — but via *your own authenticated Codex CLI*, the same path and
  terms you already accept when running Codex directly. The plugin introduces
  **no new destination or credentials**. If you're comfortable running `codex`
  in a repo, the plugin's exposure is equivalent.
- **Autonomy — configure deliberately:**
  - The plugin ships a **Stop hook** that (when the review gate is on) can
    auto-run a Codex review when Claude finishes and **block Claude from
    stopping** until issues are addressed. Decide if you want that on.
  - `/codex:rescue` and `--background` run Codex **autonomously**. What Codex
    can do to your files is governed by **your Codex sandbox/approval mode**
    (not the plugin) — set it appropriately before using these.
- No third-party telemetry/endpoints found beyond the local Codex → OpenAI path.
  (A line-by-line audit of every hook/script was not performed.)

## Verdict

**Recommended if you already use both Claude Code and Codex.** First-party,
open source, standard auditable install, and it adds no new attack surface
beyond your existing Codex setup. This is the one third-party-looking tool in
the batch that clears the bar — because it's actually first-party and reuses
trust you've already extended to Codex.

## Related

- [Leverage Subagents and Skills with Reduced Models](../agents/claude/reduced-models-for-subagents-and-skills.md)
  — the same "match the work to the cheapest capable executor" idea, across
  models instead of vendors.
- [Keeping CLAUDE.md and AGENTS.md in Sync](syncing-claude-md-and-agents-md.md)
  — share instructions across the two agents you're orchestrating.

## Sources

- https://github.com/openai/codex-plugin-cc
- https://community.openai.com/t/introducing-codex-plugin-for-claude-code/1378186
