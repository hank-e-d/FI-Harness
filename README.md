# FI-Harness

**Genre-specific game harnesses for AI-assisted game making.**

FutureIndustries approach: the harness is the product; AI fills constrained slots. Without a capable, game-type-specific harness, AI thrash is the default.

## Thesis

| Wrong order | Right order |
|---|---|
| "Make a cool RPG with AI" | Pick genre → freeze loop → scaffold harness → fill with AI → gate → iterate |
| Free-form generation until it "looks fun" | Constrained slots + automated playtests + style contracts |
| One mega-agent for everything | Specialists behind a harness that defines *what done means* |

A **harness** is the playable machine an agent is allowed to decorate — not a thin template:

1. **Runtime skeleton** — boots, loads a level, accepts input, ticks, ends
2. **Loop contract** — what the player does every 5–30 seconds (and win/lose)
3. **Data schemas** — entities, stats, items, dialogue as typed data, not prose
4. **Extension points** — named folders/APIs where AI may add content
5. **Asset contracts** — sizes, naming, pivot, palette, sheet layout
6. **Verification gates** — headless play, screenshot rubric, build-check
7. **Fence** — mechanics/economy rates frozen until art/UX pass

## Docs

| Doc | Contents |
|---|---|
| [docs/00-thesis.md](docs/00-thesis.md) | Why harness-first; lessons from Immortal Shores |
| [docs/01-architecture.md](docs/01-architecture.md) | Four layers (intent → harness → fill → verify) |
| [docs/02-harness-checklist.md](docs/02-harness-checklist.md) | What a capable harness must contain |
| [docs/03-genre-catalog.md](docs/03-genre-catalog.md) | Genre packs by tier (A / B / C) |
| [docs/04-pipeline.md](docs/04-pipeline.md) | End-to-end pipeline for one game |
| [docs/05-agent-model.md](docs/05-agent-model.md) | Conductor + specialists + non-negotiables |
| [docs/06-roadmap.md](docs/06-roadmap.md) | Build milestones (kernel → packs → Studio) |
| [docs/07-success-metrics.md](docs/07-success-metrics.md) | Metrics and anti-patterns |
| [docs/golden-rules-ai-gamedev.md](docs/golden-rules-ai-gamedev.md) | Field notes: AI gamedev golden rules (~Jun–Aug 2026) — specs, assets, loops, ship |
| [docs/research/2026-08-agent-harness-golden-rules.md](docs/research/2026-08-agent-harness-golden-rules.md) | Field notes: **agent harness / MCP / skills / agentic loops / eval / long-run failures** |
| [docs/research/2026-08-agent-workflows-emerging-consensus.md](docs/research/2026-08-agent-workflows-emerging-consensus.md) | Field notes: **emerging consensus** — systems-first, repo-as-memory, maker≠checker, bound autonomy |
| [docs/research/2026-08-specs-agents-can-build-from.md](docs/research/2026-08-specs-agents-can-build-from.md) | Field notes: **specs agents can build from** — vertical slice, GDD hierarchy, precision, non-goals |
| [docs/research/2026-08-research-pass-four-topics.md](docs/research/2026-08-research-pass-four-topics.md) | **Research campaign report** — harness + specs + assets + ship (Jun–Aug 2026) |
| [docs/research/2026-08-visual-audio-asset-pipelines.md](docs/research/2026-08-visual-audio-asset-pipelines.md) | Field notes: **visual & audio assets** — LoRA/style lock, contracts, in-game QA |
| [docs/research/2026-08-hosting-publishing-marketing.md](docs/research/2026-08-hosting-publishing-marketing.md) | Field notes: **hosting / publish / market** — itch vs Steam, trailers, disclosure |
| [docs/research/2026-08-shipping-ai-assisted-games.md](docs/research/2026-08-shipping-ai-assisted-games.md) | Field notes: **shipping AI games** — release ladder, demos, creators, funnels, postmortem numbers |
| [docs/research/2026-08-sources.md](docs/research/2026-08-sources.md) | Primary Reddit + X + paper sources for harness research |

## Packs (planned)

See [packs/README.md](packs/README.md). Shared kernel first, then genre packs:

1. **Arcade score-attack** (Tier A — build first)
2. Tiny dungeon or visual novel graph
3. Wire into FutureIndustries Studio export
4. 3D / HD-2D packs only after A–B gates are boringly reliable

## Stack context

| Concern | Tooling |
|---|---|
| Engine | Godot MCP / browser canvas |
| 3D kits | Blender MCP + Meshy |
| 2D gen | Imagine + game-asset skills |
| Browser E2E | Playwright / Scenario |
| Memory | Gestalt |
| Docs | Context7 |
| Ship (FI Arcade) | Build-check, human admin gate |

## Multi-agent house rules

Multiple agents write here. Keep the tree tidy (see also [AGENTS.md](AGENTS.md)):

| Path | Purpose |
|------|---------|
| `docs/0N-*.md` | Core design docs (maintainers / design agents) |
| `docs/research/` | Dated research digests |
| `docs/design/` | Extra design scratch that may graduate to numbered docs |
| `docs/experiments/` | Run logs, eval results |
| `notes/` | Ephemeral scratch (OK to delete) |
| `packs/` | Genre pack specs |

**Do:** dated research files, link instead of duplicate, cite sources.  
**Don't:** overwrite another agent's dated note wholesale; dump chat logs in the root; commit secrets.

## Related

- FutureIndustries platform — design on site → export pack → build with AI → publish
- FI Dev Studio — full MCP stack for agent game building
- FI Build Pack — craft docs shipped in every creator zip

## Status

**Research intake in progress.** Multi-vendor agents land dated digests under `docs/research/`.

- Core design spine (`docs/0N-*.md`) and early planning notes are in place
- Kernel and packs are **not** implemented yet
- **No synthesis / skill extract / product wire until research intake is called done**
- Nexus and other cockpits are **out of scope for now**

Session parking notes (not canon): [notes/2026-08-03-session-fi-harness-direction.md](notes/2026-08-03-session-fi-harness-direction.md)
