# Golden Rules: Visual (and audio) assets for AI-assisted game dev

- **Window:** June – early August 2026  
- **Focus:** Pipelines that shipped playable demos — generation, consistency, critique loops, engine integration, quality traps  
- **Sources:** r/aigamedev ship posts, X builders, related indie practice  
- **Related:** [Four-topic research pass](2026-08-research-pass-four-topics.md) · [Workflows consensus](2026-08-agent-workflows-emerging-consensus.md)

---

## Thesis

> **Consistency is a systems problem, not a better single prompt.** Successful builders freeze style, generate in batches with shared references, convert to engine contracts, and judge assets **in motion inside the game**.

---

## Golden Rule 1: Separate the art pipeline from the coding agent

**Rule:** Do not default to “Claude/Codex, draw me sprites.” Use dedicated image/3D/audio tools (or a specialized asset agent), then hand a clean folder to the code agent.

**Why it works:** Coding agents invent inconsistent procedural art, wrong sizes, and non-sheet layouts. r/aigamedev late Jul 2026: explicit advice to use AI-generated art assets instead of letting coding agents generate art programmatically. Board-game→video conversion case: GPT for art inventory + artwork batch, Opus for code, free SFX packs on disk — parallel tracks, then wire-in.

**Patterns / tools:** Separate “art night” sessions; export folders (`/art/raw`, `/art/engine`); Spriterrific / AutoSprite-class tools for character→sheet; Meshy/Tripo/AssetHub for 3D kits; Suno (or similar) for music stems.

**Confidence:** High

---

## Golden Rule 2: Lock style before volume

**Rule:** Commit a style bible (palette, line weight, lighting, camera, pixel density / PPU, silhouette rules) and/or train a **LoRA / style adapter** before generating the library.

**Why it works:** Random Midjourney/Flux prompts produce a mood board, not a game. dilum14 “asset consistency across entire world” (r/aigamedev, early Aug 2026): months of work; consistency from **LoRA** trained on a narrow HD-pixel style + structured world export (not one flat image). X: style preservation across campaigns framed as **cost of inconsistency**; multi-reference generation (~many refs per batch) discussed as structural fix.

**Patterns:**
- 5–20 hero reference images in every prompt  
- Fixed palette hex list  
- Forbidden: photoreal mixed with pixel; new lighting each gen  
- LoRA for house style; multi-view for 3D characters (Tripo 3.1 multi-view → assemble)

**Confidence:** High

---

## Golden Rule 3: Design engine contracts first (size, sheet, pivot, naming)

**Rule:** Before generation, define: frame size, sheet layout, pivot/origin, max colors (if pixel), file naming, import path, collision expectations.

**Why it works:** Beautiful PNGs that don’t snap to tiles or atlases force endless agent rework. X (Jul 2026): custom pipeline packing PNGs → texture atlas + JSON per sprite for load. AutoSprite-class tools advertise export for Unity/Godot/Phaser/etc. — still need your PPU and pivot rules in project docs.

**Practical contract snippet:**

```text
Player walk: 32x32, 8 frames, pivot bottom-center, sheet horizontal
Palette: palette_v1.png only
Import: res://art/characters/{id}/{anim}.png
```

**Confidence:** High

---

## Golden Rule 4: Prefer “programmatic identity + bake sheets” for animation when possible

**Rule:** For complex multi-frame characters, maintain a **re-promptable character model** (or single locked base image + pose editor); treat sprite sheets as a **baked export**, not the source of truth.

**Why it works:** Frame-by-frame AI sheets drift identity. X (Jul 2026): “don’t go for sprite sheets as source — build a model you re-prompt for consistent updates; sheet is optimization.” AutoSprite / Spriterrific: one character image → walk/attack/idle sets.

**Confidence:** Medium–High (emerging tools; still need manual QC)

---

## Golden Rule 5: World consistency needs layers, not one flat bake

**Rule:** If generating whole scenes, export **layered** data: background, occluders as separate sprites, collision/mask metadata — so the player can walk behind objects.

**Why it works:** dilum14 workflow: not a single image; CV/segmentation (e.g. SAM-class) to cut obstacles; collision layer; animations inherit parent collision. Open-source companion engine (capybara_2d_engine) because baked lighting/shadows need matching runtime.

**Trap:** Title-level “consistent world as one image” looks great in trailer, fails for gameplay depth sorting.

**Confidence:** High for 2D scene pipelines

---

## Golden Rule 6: Review assets in-game with a narrow rubric

**Rule:** Judge art under game camera, lighting, scale, and motion — not full-screen gallery crops. Rubric lists failure modes (silhouette, read at 50%, palette break, frame pop, pivot hop).

**Why it works:** Same as code: makers grade generously. Gauntlet-style contact sheets + narrow rubrics transfer to art QA. “Looks AI / noisy detail” is a known residual (dilum14 self-critique); friends who play may care more about fun than AI look — but Steam comment culture may not.

**Patterns:** Side-by-side with style bible; 1-bit silhouette check; 50% scale readability; animation onion-skin for foot slide.

**Confidence:** High

---

## Golden Rule 7: Batch by role, not by “everything in the game”

**Rule:** Generate enemies as a family, UI as a kit, tiles as a set — with shared prompts/refs — then stop.

**Why it works:** Volume without family structure guarantees mismatch. AssetHub modular character: concept → parts (multi-view 3D) → scale-match → MetaHuman/retopo → Substance — modularity for consistency.

**Confidence:** High

---

## Golden Rule 8: Pipeline to engine must be scripted

**Rule:** rembg / crop / resize / atlas / import = scripts or fixed tools. Agent only decides *what* to generate and *whether* it passes QA.

**Why it works:** Three-times rule (workflows digest): repeated ops leave the agent loop. Texture atlas + JSON exporter example on X.

**Tools:** rembg, Aseprite (incl. community AI cleanup extensions noted on r/aigamedev), custom atlas packers, Godot import presets.

**Confidence:** High

---

## Golden Rule 9: Audio follows the same contracts — generators + packs, not silence or random beeps

**Rule:** Spec audio roles early (music stems, SFX categories, loudness targets). Use dedicated music AI (e.g. Suno-class) for beds; free/paid SFX packs for UX; name files to match event IDs in code.

**Why it works:** Board-game conversion ship story: Opus produced Suno prompts while art ran in parallel; free SFX packs wired by path. Coding agents are weak at original audio and will skip or invent placeholder beeps forever.

**Patterns:**
- `audio/music/{scene}.ogg`, `audio/sfx/{event}.wav`  
- Ducking rules in design doc  
- Placeholder pack day-one so feel exists before final music  

**Confidence:** Medium (fewer detailed public postmortems than art; pattern still repeated)

---

## Golden Rule 10: Match fidelity to the product hypothesis

**Rule:** Mechanics prototypes → placeholders/primitives. Fashion/atmosphere/identity games → representative art earlier.

**Why it works:** Same as specs rule 10. Shipping “AI look” on Steam while claiming AAA fidelity invites backlash; honest simple art + strong loop ages better.

**Confidence:** High

---

## Quality traps (cheat sheet)

| Trap | Symptom | Fix |
|------|---------|-----|
| Style lottery | Every asset different lighting/era | LoRA + fixed refs + palette |
| Gallery beauty | Great stills, unreadable in-game | In-engine review at true scale |
| Sheet as source | Identity drift each frame | Base model + bake sheet |
| One mega-image world | No occlusion / collision | Layered export + masks |
| Agent-drawn art | Inconsistent sizes, no pivots | Dedicated gen + contracts |
| Silent game | Feels unfinished | SFX pack day one |
| Over-detail AI noise | “AI slop” look | Style train for cleaner look; post process |

---

## Minimal asset pipeline (recommended)

```text
1. Style bible + palette + size/pivot contracts (docs)
2. Hero references (manual pick)
3. Optional LoRA train on locked style
4. Batch generate by family (chars / tiles / UI)
5. Scripts: rembg, resize, atlas, name
6. Import to engine with presets
7. In-game QA rubric (motion + scale)
8. Fix or regenerate only failures
9. Freeze kit; code agents only wire paths
```

For 3D: multi-view mesh gen → proportion guide → retopo/UV (human or specialist) → bake/texture → rig; don’t skip topology if animation is required (AssetHub + MetaHuman class workflows).

---

## Tools named in window (non-exhaustive, high churn)

| Role | Names heard |
|------|-------------|
| 2D sprites / sheets | Spriterrific, AutoSprite, Flux/local, LoRA trains |
| 3D | Tripo, Meshy, AssetHub, MetaHuman, ZBrush/Maya/Substance (polish) |
| World consistency platform | dilum14 / Capybara-class (LoRA + layered world + open engine) |
| Atlas / pipeline | Custom PNG→atlas+JSON scripts |
| Pixel cleanup | Aseprite + community extensions |
| Music | Suno-class (prompts from coding agent OK) |
| SFX | Free packs on disk; event-named files |

Treat tool brands as examples; **rules outlast products**.
