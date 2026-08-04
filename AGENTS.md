# Agent conventions (FI-Harness)

## Current phase (2026-08-03)

**Intake CLOSED by the owner. Synthesis landed; awaiting alignment.**

Read `docs/canon/00-SYNTHESIS-AND-PRODUCT-OUTLINE.md` before proposing anything. It supersedes the
standing "do not synthesize" instruction below, and it contains findings that contradict the
`docs/0N-*.md` spine — notably that the gate-centric reading was tested and **refuted**, and that the
research corpus is only 9-of-84 verified-primary.

Still in force: do **not** start kernel/pack implementation or Nexus integration. The design is a
proposal for owner alignment, not an approved build.

Parking context: `notes/2026-08-03-session-fi-harness-direction.md` (not canon, now historical).

## Before you write

1. Read `README.md` for thesis and folder map.
2. Check `docs/research/README.md` and existing `docs/0N-*.md` so you don't duplicate.
3. Prefer additive edits: new dated files or clearly labeled sections.

## File map

| Path | Purpose |
|------|---------|
| `docs/0N-*.md` | Core design series — don't renumber casually |
| `docs/golden-rules-ai-gamedev.md` | Product-facing gamedev golden rules |
| `docs/research/` | Dated research digests (agent harness, eval, etc.) |
| `docs/design/` | Design scratch that may graduate into `0N` docs |
| `docs/experiments/` | Run logs, eval results |
| `notes/` | Ephemeral (OK to delete) |
| `packs/` | Genre pack specs |

## File naming (research / experiments)

- Research: `docs/research/YYYY-MM-short-slug.md`
- Experiments: `docs/experiments/YYYY-MM-DD-short-slug.md`
- Scratch only: `notes/`

## Research note format

```markdown
# Title
- **Window:** date range covered
- **Sources:** Reddit / X / papers (link a sources file)
- **Confidence:** high | medium | low
- **Related:** links to sibling docs in this repo

## Findings / Golden Rules
...
## Open questions
...
```

## Commits

- One logical topic per commit when possible.
- Message style: `research: …` / `design: …` / `docs: …` / `packs: …`
