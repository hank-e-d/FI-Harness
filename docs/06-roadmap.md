# Build Roadmap

## Milestone M0 — Shared harness kernel

**Goal:** reusable across all packs.

- Project layout convention (`systems/`, `content/`, `art/`, `tools/`)
- Logging + debug overlay pattern
- Headless / scripted play hook (Godot test scene or web frame-step)
- Content schema validator script
- Asset import checklist + naming manifest
- Agent `AGENTS.md` / craft playbook stub per pack
- Gestalt topic convention per game

**Exit:** empty pack skeleton boots; validator + autoplay hooks exist.

## Milestone M1 — First genre pack: Arcade score-attack

Why first: tiny loop, clear win metrics, art optional to fun, easy autoplay.

Deliverables:

- Player + waves + score + death
- `EnemyDef` + wave tables
- Hero-path autoplay
- STYLE-CONTRACT + one enemy sprite pipeline
- Gate: "survive 30s on seed 42" + build-check

**Exit:** AI can add a new enemy (def + art) without touching systems; all gates green.

## Milestone M2 — Second pack: Tiny dungeon or VN graph

Proves content schemas and branching/loot without open world.

**Exit:** second pack reuses kernel; only genre-specific systems diverge.

## Milestone M3 — Wire into FutureIndustries Studio

- Studio interview → picks pack → exports zip with harness + playbook + skill
- Academy springboard points at real finishable packs

**Exit:** creator export includes runnable starter + craft playbook + mandatory build-check path.

## Milestone M4 — 3D / HD-2D packs

Only after M1–M2 gates are boringly reliable.

- Visual rubric with actionable fails (densify kits, mid-iso life, readable workers)
- Blender/Meshy rituals in pack playbook
- Cap visual claim rounds in craft docs

## Milestone M5 — Godot download track (power user)

Same packs, richer systems; MCP stack (Godot, Blender) as primary agent surface.

## Recommended start order

1. Codify the kernel (layout, schemas validator, autoplay API, AGENTS ritual)
2. Build Arcade score-attack until embarrassingly solid
3. Run one full game through AI fill only (content + art), no systems rewrites
4. Only then generalize a second pack and FI Studio export

If agents cannot reliably finish a wave-based arcade game on a tight harness, they will not finish anything harder.
