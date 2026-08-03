# Agent Operating Model

```
Conductor (human or orchestrator agent)
  ├─ Harness guardian   — blocks illegal edits to systems/
  ├─ Content agent      — schemas only
  ├─ Art agent          — asset contracts + Imagine/Blender/Meshy
  ├─ QA agent           — runs gates, files defects with repro
  └─ Memory (Gestalt)   — decisions, gotchas, style locks
```

## Roles

| Role | Owns | Must not |
|---|---|---|
| Conductor | Phase order, gate decisions, fence opens | Deep art/content thrash without gates |
| Harness guardian | systems/, controllers, loop contracts | Approve silent "quick fixes" to core loop |
| Content agent | content/ tables, levels, graphs | Rewrite combat formulas |
| Art agent | textures, sprites, glTF, style contract | Change gameplay numbers via assets |
| QA agent | repro seeds, gate logs, fail dimensions | Ship on "looks fine to me" alone |

## Non-negotiables

1. **No silent harness changes** — systems/controller changes need human or explicit approval
2. **Seeded runs** — every automated play records seed
3. **Defects are structural** — "Atmosphere 2" → checklist item, not vibes
4. **Stop conditions** — max rounds per gate; then escalate strategy
5. **Credit awareness** — paid gen (Meshy etc.) confirmed before spend

## Ritual example: add enemy

1. Write `EnemyDef` JSON conforming to schema
2. Generate sprite per pack STYLE-CONTRACT (isolated, keyable bg, named file)
3. Attach in scene / spawn table
4. Run headless combat suite (seed recorded)
5. Screenshot checklist if visual gate applies
6. Log decision/gotcha if anything surprising

## Memory

Per-game Gestalt topic (or pack topic) for:

- Decisions (fence opens, art direction locks)
- Gotchas (unpickable objects, fog washing ortho, etc.)
- Conventions (UI chrome layout, path rules)
