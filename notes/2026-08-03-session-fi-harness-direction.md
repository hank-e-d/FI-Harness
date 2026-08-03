# Session notes: FI-Harness direction (2026-08-03)

**Status:** Parking only. **No concrete plans until research intake is complete.**

These notes capture a working discussion. They are not canon, not a roadmap freeze, and not a product commitment. Research agents should keep landing digests under `docs/research/`. Synthesis and productization wait until the owner calls research intake done.

---

## Already in repo (pre-session + early session)

- Core design spine: `docs/00-thesis.md` … `docs/07-success-metrics.md`
- Thesis: harness-first, genre-specific; AI fills constrained slots
- Research swarm: dated digests in `docs/research/` + `docs/golden-rules-ai-gamedev.md`
- Planned pack stubs: `packs/README.md` (planned only)

---

## Discussion summary (do not execute yet)

### Multi-vendor research → synthesis (deferred)

When research is *done*, a later pass should roughly:

1. Treat files as ranks: intake → research (dated) → canon (FI law) → product (skill/packs)
2. Research agents write Rank 1 only; do not each rewrite a “master plan”
3. One synthesizer extracts a claim register; conflicts go to human review
4. Promote into a small canon set; patch against existing FI-PLAYBOOK / pack-kit — do not spawn a parallel wiki
5. Skill and genre packs come from canon, not from raw research digests

**Not started.** Wait for all research notes to land first.

### FI productization (deferred)

Working sketch only:

- **Factory:** this monorepo (research → canon → kernel → packs)
- **Creator product:** genre packs in Studio export (runnable starter + craft rules + gates)
- **Skill:** thin agent skill loaded from canon / pack, not from research essays
- Free creator path must work without paid MCP / special cockpits

**Not started.** No Studio wire, no skill extract, no kernel impl from this session.

### Nexus (explicitly out of scope for now)

- Nexus (multi-vendor cockpit / experience) may fit later as an *optional* conductor for research or power-user pack builds
- **Decision 2026-08-03: leave Nexus out for now**
- Packs and craft law must not depend on Nexus
- Do not design Nexus project profiles, seats, or Studio “team build” mode until after research synthesis and a first pack path exist

### Harness naming (awareness only)

- **FI-Harness (this repo):** genre game harnesses / factory for AI game packs — FutureIndustries-facing
- **Harness / Nexus cockpit:** orchestrator product (separate line) — not this repo’s job for now

---

## Standing instruction until intake is called done

1. Research agents: append dated digests under `docs/research/`; update `docs/research/README.md` index
2. Do not renumber or rewrite `docs/0N-*.md` as part of research dumps
3. Do not open synthesis, claim registers, skill extraction, or pack implementation as “the plan”
4. Owner will trigger synthesis after research notes are in

---

## Related chat topics (for later retrieval)

- Immortal Shores lessons: mechanics fence first; visual gates need actionable fails; cap claim rounds
- Stack context: Godot MCP, Blender/Meshy, Imagine skills, Playwright, Gestalt, FI pack-kit
- Existing FI doctrine to reconcile later: `FI-PLAYBOOK.md`, `FI-GRAPHICS.md`, pack-kit tooling
