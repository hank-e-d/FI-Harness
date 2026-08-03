# Architecture: Four Layers

```
┌─────────────────────────────────────────────────────────┐
│  L0  Intent & product gate                               │
│  pitch, audience, session length, ship bar, AI% policy   │
└──────────────────────────┬──────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────┐
│  L1  Genre Harness (the main investment)                 │
│  loop · schemas · scenes · systems · tools · fences      │
└──────────────────────────┬──────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────┐
│  L2  Fill pipelines (AI specialists)                     │
│  code · systems · content · art · audio · narrative      │
└──────────────────────────┬──────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────┐
│  L3  Verification & publish                              │
│  unit/sim · headless play · visual judge · build-check   │
└─────────────────────────────────────────────────────────┘
```

## Layer responsibilities

### L0 — Intent & product gate

- Genre pack selection
- Fantasy one-pager + reference mood
- Session length + ship bar (demo / jam / Arcade / commercial)
- Art direction locks asset contracts (pixel / HD-2D / low-poly / flat UI)
- FutureIndustries AI% transparency when publishing to Arcade

### L1 — Genre harness

The main engineering investment. Includes runtime skeleton, loop contract, data schemas, extension map, asset contracts, verification hooks, and mechanics fence. See [02-harness-checklist.md](02-harness-checklist.md).

### L2 — Fill pipelines

Specialist agents (or human+AI) fill named slots only:

| Lane | Allowed work |
|---|---|
| Content design | Tables, levels, spawn defs within schemas |
| 2D art | Sprites/tiles/UI per style contract |
| 3D art | Kits/props per mesh budget + empties list |
| Narrative | Dialogue/quests on graph schema |
| Audio | SFX/music loops |
| Systems | **Rare** — only when harness has a proven gap |

### L3 — Verification & publish

Automated gates, then build-check / package / human play before publish.

## Hard rule

**L2 may not change L1 contracts without an explicit "harness PR."** Agents that rewrite the loop mid-content-pass are the common death spiral.
