# Pre-registration — signed before Phase 0

- **Signed:** 2026-08-06, by the owner, before any game code exists.
- **Why this file exists:** Immortal Shores authored its acceptance criteria three times *during* the build and the score fell every time (8.0→6.6, then 7.6→7.1). Terms signed after the fact get reinterpreted. These are signed before.
- **Governs:** `02-TRIAL-PROTOCOL.md`

---

## 1. Rulings

### R1 — The no-terminal rule constrains the human, not the agent. **SIGNED.**

> *"Every terminal action becomes a UI element"* means **the creator never types a command.** Their agent may execute anything inside the exported folder.

**Consequences, now binding:**
- `tools/` ships **populated**, not as the empty string `js/export-pack.js:1179` currently emits.
- Exit codes, seeded headless bots, determinism checks, a mutation probe and a pre-commit hook written by the export step are **all pack-shippable with zero installs.**
- Six of the ten "studio-only" capabilities collapse into the pack. **The single-product plan in `01-BUILD-PLAN.md` proceeds as written.**
- The three that genuinely don't fit stay on the bench: Playwright's Chromium download, memory spanning games, the Godot track.

### R2 — If gate-block count is 0, the run is VOID and gets re-run with tighter assertions. **SIGNED.**

A zero means the gates sat below where the work naturally lands — the treatment was **not administered**, again. **A smooth build with a zero is not a win.** It is the same non-administration that makes the last six repos uninformative.

*Not chosen, recorded for honesty:* the alternative was to accept that verification isn't the lever and redirect to finishing. That option remains available **only after a re-run also returns zero.** Two zeros is a finding.

### R3 — The theme is salvaged from a dead project. **SIGNED. Immortal Shores confirmed by the owner 2026-08-06.**

### R4 — "Shipped" means **live in the FI Arcade catalog.** Recorded by default; override before Phase 0 ends.

Live-in-catalog is measurable inside the trial window and is in the owner's control. *Played-by-a-stranger* is the more honest finish line but depends on someone else showing up, which would make the 72-hour FALSIFIED clock unmeasurable. Chosen for measurability, not because it is the better definition of done.

---

## 2. Pre-registered outcomes

| Verdict | Condition |
|---|---|
| **CONFIRMED** | Live on the Arcade within 3 sessions · gate-block **≥3** · RTVA **≤3** · mutation guard reports grip |
| **VOID** | gate-block **= 0**, regardless of how the game turned out → re-run with tighter assertions (R2) |
| **FALSIFIED** | Bot-verified playable, gates green, grip confirmed, RTVA in budget — **and still unshipped 72 hours later** |
| **SECONDARY FALSIFIED** | RTVA hits 5, **or** median HMAI exceeds 20 minutes → painters did not deliver recoverability; pull that pillar and reconsider it independently |

**FALSIFIED means stop building harnesses.** It would say the failure mode is upstream of both verification and art recoverability, and every further hour on gates treats the wrong disease. The entire 84-rule canon is downstream of the assumption that verification is the lever.

**"Shipped" is not yet defined** — live-in-catalog vs played-by-a-stranger. **Decide it before Phase 0 starts;** the 72-hour clock hangs off it.

---

## 3. The project — recommendation

> **Salvage the Immortal Shores fiction. Build it at CURFEW scale.**

### Why this corpse

1. **The numbers are already pinned, and that is the whole point.** `packages/shared/src/rates.ts` opens *"GDD rate tables — single source of truth"* and exports real constants: `STARTER_WORKERS = 18`, `STARTER_RATIONS = 60`, `STARTER_MUDBRICKS = 40`, `SEAL_FLOOR = 10`, `WORKER_GROWTH_PER_HOUR = 3`, `RATION_UPKEEP_PER_WORKER_HOUR = 1`, `BARGE_CAPACITY = 100`. **VERIFIED.** Those become `sim-check.mjs` assertions on day one — which is exactly Phase 1 step 8, and exactly what gave CURFEW's suite grip (it pins damage at 6/9/12/15/24).
2. **`buildingCatalog.ts` is 11.8KB across 15 building kinds** — the 15–25 entity budget is already designed, named and costed.
3. **The fiction is architectural and systematic**, which is precisely what `FI-GRAPHICS.md` says to draw in code. Zero external assets is the natural choice here, not a sacrifice.
4. **It is the canonical failure.** A finished descendant is the strongest result this trial can produce.

### This is a NEW GAME. Nothing is edited, branched, forked or copied.

**Zero lines of Immortal Shores code move.** No Babylon.js, no Fastify, no npm workspaces, no TypeScript build, no `.glb`, no `station.js`. Immortal Shores is untouched and stays where it is.

**Exactly three things carry, all of them retyped by hand:**

| Carries | What it is |
|---|---|
| The **fiction** | Nile settlement, mudbrick, emmer, rations, seals, workers, harbor, great house — vocabulary, not code |
| ~15 **numbers** | `STARTER_WORKERS = 18`, `STARTER_RATIONS = 60`, `SEAL_FLOOR = 10`, `RATION_UPKEEP_PER_WORKER_HOUR = 1` … as plain JS constants |
| 15 **entity names** | the building kinds from `buildingCatalog.ts` |

The value is not the code. It is that **someone already tuned those numbers**, so `sim-check.mjs` has real assertions to pin on day one instead of balance being invented mid-build.

This does **not** violate the standing no-duplicate-game-folders rule: that rule forbids copying a game in order to iterate on it. This is a different game.

### Where it lives

```
GameMaking/<new-name>/     NEW folder · git init · ITS OWN pre-commit hook
  README.md  ACCEPT.md  LOOKBOARD.html  METRICS.tsv
  tools/     4 scripts, 400 lines, frozen after Phase 0
  play/      the game, ~3-4k lines, written from scratch
```

**Its own git repo, not the FI repo.** The hook is the entire experiment and hooks are per-repo; putting the game inside `FutureIndustries/` would gate every unrelated FI commit on this game's tests.

At ship time `play/` copies into `FutureIndustries/games/<name>/` with a manifest and an html wrapper — the same path CURFEW took. (Verified: `GameMaking/curfew/` is not a git repo at all, and the live copy sits at `FutureIndustries/games/curfew/` + `games/curfew.html` + `games/manifests/curfew.json`.)

### The one honest conversion

`rates.ts` says *"All production is per assigned Worker per real hour."* That is the MMO clock. **The descendant is per TURN.** One conversion, applied once, at Phase 0. The other numbers carry unchanged.

### THE SHAPE IS LOCKED — this is where the risk lives

The named risk of salvage is **scope gravity**: the original was an isometric MMO city builder with a server, trade, chat, a world map and four settlements. **The descendant is none of that.**

| Original | Descendant |
|---|---|
| Isometric 3D, Babylon.js, 16 hand-exported `.glb` | **One screen. Painters. Zero asset files.** |
| Real-time hours, offline catch-up | **Discrete turns** |
| Fastify server, two-account trade, chat | **No server. No multiplayer. No network.** |
| 4 settlements, world map, monuments | **One settlement** |
| 3,218 lines of imperative render code | **No file over 900 lines or 35% of the codebase** |

**One settlement · N turns · assign workers · one win condition · one loss condition · 15–25 entities · ~3–4k lines · 3 sessions hard cap.**

If Phase 0 finds itself designing a world map, the salvage has failed and the shape wins.

---

## 4. The economy is already tuned — this is why the salvage is worth it

Verified in `packages/shared/src/rates.ts`:

| Building | Output | Rate / worker / turn | Input |
|---|---|---|---|
| `emmer_field` | emmer | **8** | — |
| `ration_house` | rations | **6** | 2 emmer per output |
| `river_clay_pit` | river_clay | **5** | — |
| `marsh_reed_bed` | marsh_reeds | **5** | — |
| `mudbrick_yard` | mudbricks | **2** | — |

Plus `STARTER_WORKERS = 18`, `STARTER_RATIONS = 60`, `STARTER_MUDBRICKS = 40`, `RATION_UPKEEP_PER_WORKER_HOUR = 1`, `WORKER_GROWTH_PER_HOUR = 3`, `SEAL_FLOOR = 10`, `STARTER_SEALS = 10`.

**The tension falls out of the numbers, not out of a design session.** 18 workers eat 18 rations a turn against a 60-ration buffer — three turns of slack. Feeding the settlement takes ~3 ration-house workers, which needs ~4.5 emmer-field workers. **About 8 of 18 workers exist only to keep the other 10 alive.** That is a real economy and none of it has to be invented.

Every one of these numbers becomes a `sim-check.mjs` assertion in the same commit as the rule it pins.

---

## 5. Proposed premise — for owner approval at Phase 0 step 1

> **Eighteen workers. Every turn they eat. Assign them across the settlement and raise the monument before the rations run out.**

- **Loop (one turn):** assign workers → production resolves → every worker eats 1 ration → check win/lose
- **Win:** the monument completes (a fixed mudbrick cost)
- **Lose:** rations reach 0
- **Entities:** the 15 building kinds from `buildingCatalog.ts` — already inside the 15–25 budget
- **One screen.** The settlement is a list or a grid of plots, not an isometric world.

Not approved yet. Step 1 of Phase 0 is the owner accepting, editing, or replacing this sentence.

---

## 6. Still open before the clock starts

Nothing blocking. R1–R4 are signed and the project is confirmed.

Phase 0's two-hour cap covers: `README` one-liner · `ACCEPT.md` frozen · Look Board (≥10 min, a typed note on every row) · four scripts, 400 lines, deliberately bad · hook wired and **personally watched refusing a commit** · `METRICS.tsv` `t0`.

**Division of labour for Phase 0:** steps 1–2 and 4–6 can be drafted and built for the owner to approve. **Step 3, the Look Board, is the owner's and cannot be delegated** — the picks and the typed notes are the entire information content, and a delegated board is the 12.6-second rubber stamp again.
