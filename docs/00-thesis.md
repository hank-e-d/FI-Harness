# Thesis: Harness-First AI Game Making

## Core claim

**The harness isn't just first — it's the product.** AI is strong at filling constrained slots (code, art, levels, copy) and weak at inventing a stable game *shape* from a vibe. A capable, game-type-specific harness is the shape; everything else is fill.

## Why not free-form

| Approach | Outcome |
|---|---|
| Free-form generation until it "looks fun" | Claim rounds, no ship gate |
| One mega-agent owns everything | Illegal systems rewrites mid-content |
| Universal mega-template for all genres | Wrong tick models, wrong verify, weak constraints |
| **Genre harness + specialists + gates** | Checkable work, fenced mechanics, finishable games |

## Failure mode already observed (Immortal Shores)

- Mechanics fence held; rates/plots/trust trade protected
- Visual gauntlet spun multiple rounds; terminal judge stuck (e.g. Atmosphere=2)
- Ship gate closed not because agents can't paint, but because "premium 3D browser game" without densified kits + mid-field readability + actionable failure modes becomes endless claim rounds

**Lesson:** gates need *actionable* failure modes ("Atmosphere=2 → add life props on empties"), not only OVERALL PASS/FAIL. Cap visual claim rounds; change strategy, not prompt count.

## Why harness-by-type (not one universal harness)

Genres disagree on almost everything that matters to agents:

| Dimension | Turn-based RPG | Arcade shooter | Sim / city | Platformer | Visual novel |
|---|---|---|---|---|---|
| Tick model | Discrete turns | Fixed physics dt | Slow day/tick | Physics + coyote | Event graph |
| "Good" content | Balance tables, moves | Spawn waves, patterns | Rules + UI affordances | Level geometry | Script + branches |
| Hard AI failures | Broken formulas | Unfair bullets | Dead UI / unpickable | Softlocks | Orphaned flags |
| Best verify | Combat sims | Frame-step + hit tests | Click-path E2E | Path reachability | Graph coverage |
| Art load | Characters + tiles | FX + enemies | Kits + icons | Tiles + anims | CGs + UI |

A **genre pack** is the unit of investment. The universal layer stays thin: project layout, logging, export, agent rituals, memory hooks.

## Bottom line

AI does not replace game design. It fills a machine you already defined — with contracts, fences, and gates that turn "make it better" into checkable work.
