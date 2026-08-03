# Genre Packs

Packs live here as they are implemented. Each pack is a complete harness fork: loop, schemas, systems, extension map, asset contracts, gates, and agent playbook.

## Planned order

| ID | Pack | Tier | Status |
|---|---|---|---|
| `arcade-score` | Arcade score-attack | A | Planned |
| `dungeon-tiny` | Tiny top-down dungeon | A | Planned |
| `vn-graph` | Visual novel / choice graph | A | Planned |
| `match-grid` | Match / puzzle grid | A | Planned |
| `platformer` | Side-view platformer | B | Later |
| `tactics-rpg` | Turn-based tactics battles | B | Later |
| `idle` | Idle / incremental | B | Later |

## Pack layout (target)

```text
packs/<id>/
  README.md              # loop doc + ship bar
  AGENTS.md              # extension map + rituals
  STYLE-CONTRACT.md      # art rules
  schemas/               # machine-readable content shapes
  systems/               # 🔒 harness-owned runtime
  content/               # ✅ AI-fill slots (starter samples)
  art/                   # placeholders + import presets
  tools/                 # validators, autoplay, gates
  tests/                 # hero path, balance smoke
```

## Kernel

Shared kernel patterns will land under `/kernel` (or be documented in M0) once implementation starts. Packs should depend on the kernel rather than copy-paste core validators and autoplay hooks.
