# Capable Harness Checklist

A pack is not "ready for AI fill" until every section below is present and green.

## 1. Playable vertical slice (day-zero)

- [ ] Boots to a title → one real level → win and lose both reachable
- [ ] One "hero path" scripted for automated play (not only human play)
- [ ] Debug overlays: state dump, collision, spawn points, FPS
- [ ] Seeded RNG for reproducible playtests

## 2. Loop document (1 page, frozen)

- [ ] Core verb(s)
- [ ] Risk / reward / progression
- [ ] Session length target (e.g. 3 min arcade / 20 min RPG chapter)
- [ ] Explicit v1 out-of-scope list

## 3. Content schemas (machine-readable)

Prefer JSON / YAML / Godot resources over free-form markdown for anything the runtime loads. Markdown is for humans; schemas are for agents.

Examples:

```text
EnemyDef  { id, hp, speed, attacks[], dropTable, spriteRef }
RoomDef   { id, exits, spawns[], goals[] }
DialogueNode { id, speaker, text, choices[] → next }
WaveDef   { id, enemies[], interval, scoreBonus }
```

- [ ] Schema files validated by a script
- [ ] Runtime loads only validated content
- [ ] All refs resolvable (integrity gate)

## 4. Extension map ("AI may touch")

Example:

```text
res://content/enemies/      ✅ add defs + art
res://content/levels/       ✅ add rooms from room grammar
res://systems/combat/       🔒 harness only
res://player/controller.gd  🔒 harness only
```

- [ ] Written map in pack `AGENTS.md` / craft playbook
- [ ] CI or guardian script fails on illegal path edits (optional but recommended)

## 5. Asset contracts

Bind engine-ready defaults (see game-asset skills) into the pack:

| Need | Contract |
|---|---|
| Character/prop sprite | Isolated subject, flat keyable bg, clean silhouette |
| Anything that moves | Frame sequence that loops; video-first pipeline OK |
| Sprite sheet | Uniform cells, no divider lines, stable subject position |
| Ground / terrain | Seamless tileable; verify 2×2 composite |
| UI panels / buttons | 9-slice friendly; no baked text (localize) |
| Same character again | Edit-chain from base image, never regen fresh |
| Icons | One style contract; legible at 32px |

Plus engine-specific:

- [ ] Import presets / pixel snap
- [ ] Collision from alpha where applicable
- [ ] Max texture size / mesh budget
- [ ] Naming manifest for generated files

## 6. Tooling surface for agents

Document the ritual, not only the tools:

> add enemy = write def → generate sprite per contract → attach in scene → run headless combat suite → screenshot checklist

| Concern | Typical tool |
|---|---|
| Engine structure | Godot MCP (scenes, nodes, run, debug) |
| 3D kits / scenes | Blender MCP + Meshy |
| 2D gen + edit | Imagine + game-* skills |
| Browser E2E | Playwright (Scenario when available) |
| Memory across sessions | Gestalt |
| Library docs | Context7 |
| Ship gate (FI Arcade) | Build-check; no blank index |

- [ ] Pack includes craft playbook + agent snippet
- [ ] Credit-aware note for paid gen (Meshy etc.)

## 7. Gates (must be automatic)

| Gate | Pass condition |
|---|---|
| Boot | Project runs; main scene loads; no errors in log |
| Play | Scripted path reaches win in ≤N steps |
| Balance smoke | Combat sim: not dead in 1 turn; not invincible |
| Content integrity | All refs resolve; no missing textures |
| Visual floor | Rubric ≥ threshold; **named** failure dimensions |
| Build-check | Real game entry; not empty reskin / blank screen |

Visual gates specifically:

- [ ] Rubric dimensions named (readability, life, contrast, chrome, atmosphere, …)
- [ ] Each fail maps to an action (e.g. densify kits on empties)
- [ ] Max claim rounds (e.g. 3) then strategy change required

## 8. Mechanics fence

- [ ] Rates, damage, economy knobs locked before content/art flood
- [ ] Opening a fence requires explicit decision (logged)
