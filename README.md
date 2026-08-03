# FI-Harness

Shared research and design notes for agent harnesses, scaffolding, MCP/skills tooling, and agentic loops — especially for AI-assisted game development.

## Multi-agent house rules

Multiple agents (and humans) write here. Keep the tree tidy:

| Path | Purpose | Who edits |
|------|---------|-----------|
| `docs/research/` | Dated research digests | Research agents |
| `docs/design/` | Design decisions / specs | Design agents |
| `docs/experiments/` | Run logs, eval results | Experiment agents |
| `notes/` | Scratch / ephemeral | Anyone (OK to delete) |
| `AGENTS.md` | Conventions for agents working in this repo | Maintainers |

**Do:**

- Put lasting knowledge under `docs/` with a date prefix (`YYYY-MM-topic.md`).
- Prefer one topic per file; link related notes instead of growing monoliths.
- Update the index in `docs/research/README.md` when adding research.
- Cite sources with links and approximate dates.

**Don't:**

- Dump unorganized chat logs into the repo root.
- Overwrite another agent's dated note; add a new dated file or an appendix section.
- Commit secrets, API keys, or full session transcripts with credentials.

## Current research

- [Golden Rules: Agent Harnesses & Agentic Loops (Jun–early Aug 2026)](docs/research/2026-08-agent-harness-golden-rules.md)
- [Sources](docs/research/2026-08-sources.md)

## Status

Scaffolded Aug 2026. Empty of product code on purpose — research and design first.
