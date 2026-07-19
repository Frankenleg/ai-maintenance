# AI Maintenance

Documentation for managing and maintaining AI coding agents.

## Organization

- **[`agents/`](agents/)** — docs specific to a single agent.
  - **[`agents/claude/`](agents/claude/)** — Claude / Claude Code.
- **[`shared/`](shared/)** — cross-cutting docs that apply to any agent.

Each folder has its own `README.md` index. Images live in an `images/`
subfolder next to the docs that use them.

If a topic starts agent-specific and later proves cross-cutting, move it into
`shared/` and update the links.
