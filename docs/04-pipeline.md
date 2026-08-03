# End-to-End Pipeline (One Game, One Harness)

## Phase 0 — Product intent (human, short)

- Genre pack selection
- Fantasy one-pager + reference mood
- Session length + ship bar
- Art direction → locks asset contracts

## Phase 1 — Harness selection or fork

- Start from genre pack, not empty engine project
- Freeze loop doc + schemas
- Confirm hero-path automated play passes on *empty content*

## Phase 2 — Vertical slice (human + AI code agent)

- One complete "fun unit" (one room, one wave, one match-3 level)
- Placeholder art OK if contracts are correct sizes
- Mechanics fence: rates, damage, economy locked

## Phase 3 — Content fill (parallel specialists)

| Lane | Agent job | Inputs | Outputs |
|---|---|---|---|
| Systems | Only if harness gap | loop doc | PR to systems (rare) |
| Content design | Fill tables/levels | schemas + balance fence | JSON/resources |
| 2D art | Sprites/tiles/UI | style contract + refs | engine-ready files |
| 3D art | Kits/props | mesh budget + empties list | glTF |
| Narrative | Dialogue/quests | graph schema | scripts |
| Audio | SFX/music loops | free tools or gen | ogg/wav |

**Orchestration:** one conductor owns gates; specialists do not rewrite each other's lanes.

## Phase 4 — Integrate & fence

- Import assets → wire refs → content integrity
- Re-run hero path + balance smoke
- Only then open mechanics knobs if play feels wrong

## Phase 5 — Visual / feel gauntlet

- Named rubrics (readability, life, contrast, chrome, atmosphere)
- Cap claim rounds (e.g. 3); then change *strategy* (densify kits, not more prompts)
- Capture before/after; log failures to memory (Gestalt)

## Phase 6 — Ship

- Build-check / Playwright smoke
- Package for Arcade or export zip with craft playbook
- AI% transparency if FI product rules apply
- Human play 15–30 min before publish
