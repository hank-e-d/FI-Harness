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
| [docs/golden-rules-ai-gamedev.md](docs/golden-rules-ai-gamedev.md) | Field notes: Reddit/X golden rules (~June–Aug 2026) across harness, specs, assets, loops, code, ship |

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

## Related

- FutureIndustries platform — design on site → export pack → build with AI → publish
- FI Dev Studio — full MCP stack for agent game building
- FI Build Pack — craft docs shipped in every creator zip

## Status

Planning notes captured 2026-08-03. Kernel and packs not yet implemented in this repo. Field research (golden rules) added 2026-08-03.
