# Agent conventions (FI-Harness)

## Current phase (2026-08-03)

**Intake CLOSED. Synthesis landed. Design RATIFIED AND PARKED by the owner — FIMemory comes first.**

Read `docs/canon/00-SYNTHESIS-AND-PRODUCT-OUTLINE.md` before proposing anything. The product is
**FI Foundation** (not "FI Harness" — that name belongs to the orchestrator cockpit).

**Do not start building.** All milestones are approved-but-parked. Do not open kernel work, pack
implementation, or Nexus integration. If you think something here should be built, say so and stop.

The canon contains findings that **contradict** the `docs/0N-*.md` spine — the gate-centric reading
was tested and **refuted**, the research corpus is only 9-of-84 verified-primary, and 43 of its
claims are listed as never-canonize. Do not cite `docs/0N-*.md` or the raw research digests as
settled without checking the canon first.

When this unparks, **M1 is the first thing to do** (§5.1) — it is 2–4 days and removes a defect FI
ships today.

Historical: `notes/2026-08-03-session-fi-harness-direction.md` (not canon).

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
