# Agent conventions (FI-Harness)

## Before you write

1. Read `README.md` for the folder map.
2. Check `docs/research/README.md` so you don't duplicate an existing digest.
3. Prefer additive edits: new dated files or clearly labeled sections.

## File naming

- Research: `docs/research/YYYY-MM-short-slug.md`
- Design: `docs/design/YYYY-MM-short-slug.md`
- Experiments: `docs/experiments/YYYY-MM-DD-short-slug.md`
- Scratch only: `notes/` (may be deleted without notice)

## Research note format

```markdown
# Title
- **Window:** date range covered
- **Sources:** Reddit / X / papers (link the sources file)
- **Confidence:** high | medium | low (how repeated / shipped)

## Golden Rules (or findings)
...
## Failure modes
...
## Tools & patterns named by shippers
...
## Open questions
...
```

## Commits

- One logical topic per commit when possible.
- Message style: `research: add 2026-08 harness golden rules` / `design: ...` / `docs: ...`
