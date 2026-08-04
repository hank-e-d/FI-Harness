# FI Foundation — synthesis of the research swarm, and the product outline

- **Status:** **RATIFIED AND PARKED (owner, 2026-08-03).** The design is approved as the plan. **Execution waits behind FIMemory.** Nothing is built and nothing should be started — see §5.1 for the one cost this accepts.
- **Name:** **FI Foundation** (owner, 2026-08-03). Not FI Harness — that is the orchestrator cockpit. Supersedes the working name "FI Floor" used in the first draft.
- **Date:** 2026-08-03
- **Supersedes:** nothing. `docs/00-thesis.md` … `docs/07-success-metrics.md` remain the prior design spine; where this document contradicts them it says so explicitly and gives the evidence.
- **Trigger:** `AGENTS.md` froze synthesis until the owner called research intake done. This document is that call.
- **Method:** three research swarms (33 agents), plus first-hand verification by the author of every load-bearing claim. Claims marked **VERIFIED** were checked against disk or the live web by the author, not by a subagent.

---

## 0. Read this part first

Three things changed shape during this synthesis. If you read nothing else, read these.

### 0.1 The obvious answer is wrong, and we tested it properly

The intuitive product — *"AI game-making is inconsistent because nothing is machine-enforced, so ship machine-enforced gates"* — was the author's working thesis for most of this exercise. **It is refuted.** An adversarial pass tried to break it and succeeded, on evidence that was then independently verified:

| Evidence | Status |
|---|---|
| P(good outcome \| strong gate) = **1/5**. P(good \| weak or no gate) = **2/3**. The correlation runs *backwards*. | from forensics |
| **CURFEW** — the portfolio's cleanest success (shipped live, complete loop, one agent session, one day, 3,694 lines) — has **no `tools/` directory, no judge, no visual gate**. | **VERIFIED** |
| **NileInSpace already ran the experiment — and the gate returned a FALSE PASS.** `tools/aaa-judge.py` is a real objective visual gate (16,697 bytes, six checks, `raise SystemExit(main())`). It reached **PASS 6/6 and held four iterations while the visuals were unshippable.** See §0.4 — this is the single most important correction in the document. | **VERIFIED** |
| A gate printing PASS was twice followed by the owner rejecting the build. **Not capriciousness — in the NileInSpace case the gate was measuring the wrong thing entirely.** | see §0.4 |
| Mutation test on AI Prompt Simulator: deleting `style.css` (2,165 lines), gutting `index.html` to `<html></html>`, and throwing at the top of `ui.js` **all left the suite at 92/92, exit 0**. | from forensics |
| **Zero `.github` directories and zero non-sample git hooks** across six game repos. The "treatment" was never administered — there was no treated group. | **VERIFIED** |

A product built on "gates cause quality" would be selling the scar, not the cause.

### 0.2 What survives is narrower and more useful: gates don't cause quality, executable criteria cause **termination**

One within-subject control survives every objection, because it holds person, day, scope, genre and model constant:

> **Immortal Shores, same human, same eight-hour sitting, two halves.**
> The **mechanics** half, whose acceptance criterion was `tools/critical-audit.mjs` with `process.exit(fail>0?1:0)` — **converged in 2h06m and stayed closed.**
> The **visual** half, whose acceptance criterion was 30 rounds of hand-written markdown — **ran 5h33m and never closed.**

Confluence repeats it across a week: v1 retired a *passing* executable gate for an unpassable prose one and produced 261 judge runs in a day, then a from-scratch rewrite; v2 replaced prose lessons with scripts that exit non-zero and hit 8/8 on three consecutive runs.

Prose criteria never terminate. They can always find one more complaint, and they rescale between rounds — one project's judge rescored every dimension 1.5–3 points lower with **no regression**. A boolean cannot do that.

**The failure mode is not bad output. It is non-termination.** These games don't fail. They never finish, and then the next one starts — six hours between 2342 going dark and CURFEW being created; 110 minutes between NileInSpace's last judge run and the Confluence rewrite.

### 0.3 The confound that beats gates outright: **art the agent cannot fix in the turn it is judged**

Every project that stalled on *look* routes art through a **binary pipeline a human must drive**:
- Immortal Shores: 16 hand-exported `.glb` from Blender.
- NileInSpace: six script versions for one midfield plate, eight for one FOV bake, with a documented trap where stale `.webp` siblings silently mask fresh `.png`.

Both projects that came out **coherent** have **zero external assets**:
- Crookemon2: 29 creatures are **218KB of painter *code***; every texture, sprite, portrait, track and SFX synthesized at runtime.
- CURFEW manifest, verbatim: *"Zero external assets. All art is original inline SVG; all audio is WebAudio synthesis. No image or sound files exist."*

When art is code, the agent sees the defect, fixes it, and re-renders **in one turn**. When art is a `.glb`, every round costs a human Blender session and the loop physically cannot close.

**`FI-GRAPHICS.md` already mandates exactly this** — systematic art drawn in code, image generation only for singular assets. The failures violated FI's own doctrine, in the same zip that ships the doctrine.

### 0.4 The false pass, and the second requirement: **recoverability**

*(Owner correction, 2026-08-03, after the first draft of this document. It replaces the reading that the owner "overruled a passing gate.")*

> **Eric:** *"it was a false fail, the visuals were so bad I couldn't recover it and had to start from scratch."*

**Verified in `tools/aaa-judge.py`.** Its six *"objective, non-negotiable gates (must all PASS for **ship**)"*:

| # | Gate | What it actually tests |
|---|---|---|
| 1 | `FPS_GATE` | frame time ≤ 16.7ms | performance |
| 2 | `DIAMOND_AUDIT` | `diamond-audit.py` exits 0 | one specific bug |
| 3 | `EMPTY_PAD_READ` | cyan/teal pixels visible in pad zones | one specific bug |
| 4 | `NO_FULLBLEED` | no sprite bottom-band fill ≥ 0.90 | one specific bug |
| 5 | `BUILDING_COVERAGE` | ≥ N art files load with solid alpha | files exist |
| 6 | `FRAME_DUMP` | money-shot PNG "is not blank/black" | screenshot isn't empty |

**Not one measures whether it looks good.** Every one is *nothing is broken / something is present*. Gate 6 only checks the image isn't black. This is a **floor check labelled as a ship gate** — and that mislabelling is the defect, not the checks themselves.

**Why it became terminal.** `js/station.js` is **5,973 of NileInSpace's 13,443 total JS lines — 44% of the codebase in one hand-written imperative render module that also owns world sim.** **VERIFIED.** So *"the visuals are bad"* meant *"rewrite six thousand lines."* Restarting from scratch 110 minutes later was the **cheaper option**, not a failure of nerve.

**Two consequences for the product, both first-class:**

1. **A green must never be readable as "good."** A floor check that fires on a broken-looking build teaches the creator the word means nothing. This is the same trap as `FINISHED (placeholder)` — the reason that framing was cut in §3.3. **Grade any proposed gate by asking what it would have said about NileInSpace on the day it was abandoned. If the answer is PASS, it is a floor check, and it must be labelled as one.**
2. **Recoverability sits beside termination as a design goal.** The benefit of *painters, not files* (§3.2, Pillar 2) is not only that the loop closes faster. It is that **bad visuals stay recoverable instead of becoming terminal.** Under role-driven painters, "it looks wrong" is a re-render. Under a 6,000-line hand-written `station.js`, it is a rewrite — and a rewrite is where projects go to die.

This correction *strengthens* Pillars 1 and 2 and it *narrows* what any future verification work may claim about itself.

---

## 1. What the research corpus is actually worth

**561 raw claims → 84 canon rules.** Evidence basis of those 84:

| Basis | Count |
|---|---|
| verified-primary | **9** |
| named-secondary | 39 |
| anecdotal | 18 |
| owner-authored (Eric's own prior drafts) | 12 |
| echo-only | 6 |

### 1.1 The citation position, stated fairly

Two docs (`2026-08-golden-rules-verified-hn-sources.md`, `2026-08-golden-rules-independent-crosscheck.md`) are scrupulously honest that **reddit.com returned 403 and x.com returned 402 for the entire run**. Four others cite Reddit threads with detailed paraphrases and specific numbers and carry no such caveat. Three checks were run:

- **Reddit is genuinely unfetchable** from this environment too. **VERIFIED.**
- **The HN citations are exact.** `news.ycombinator.com/item?id=47400868` is Show HN for Godogen at **337 points, 205 comments** — matching the doc to the digit. **VERIFIED.**
- **A Reddit-sourced headline claim checks out independently.** Fire Field is real: Naoki Fujinaga, solo, no prior gamedev experience, Diablo-like ARPG, ~3 months, shipping on Steam as *Firefield* (app 4527100). **VERIFIED.**

**Operating rule: no number from this corpus enters FI product copy or a customer-facing claim without a spot-check.** The corpus is a mix of real and unverifiable, and the mix is not labelled.

### 1.2 Never canonize these

43 items were named unusable. The ones most likely to have reached a pitch deck:

- Databricks "harness choice moves cost ~2×" / "~3× less context" — an internal eval, uncheckable by construction, two multipliers presented as one finding.
- "Single agent loop covers 80% of cases," attributed to *Building Effective Agents* — no locatable passage.
- "0 of 71 compacted rollouts passed" — no benchmark name, no link, no comparison rate. It is the most load-bearing anti-compaction number in the corpus.
- `arXiv 2607.14275` — cited as "discourse," used to underwrite four named failure modes. Nobody in the chain claims to have read it.
- Probably-hallucinated product names: **Intaris, Moltwire / arc-gate, ProofAgent-Harness, "Sol."** Also treat *Spriterrific*, *AutoSprite*, *Capybara-class*, *Tripo 3.1 multi-view* as unverified.

### 1.3 The corpus agreeing with FI is not evidence

Seven of the source docs are Eric's own planning drafts (`00-thesis` … `07-success-metrics`). Rules that look independently corroborated — *freeze the loop doc*, *Tier C is off-limits*, *arcade is the capability floor* — are **FI agreeing with FI**. Fine as product direction. Never report it as "the research found."

---

## 2. What FutureIndustries actually ships today

All **VERIFIED** on disk.

| Asset | State |
|---|---|
| `js/export-pack.js:1179` | Emits **`tools/` as an empty directory.** Only ever receives `make-sfx.py`, `env.example`, and a 991-byte markdown recipe ending *"Paste results into BUILD-CHECK.md."* Line 929 tells the creator: *"tools/ — local helper scripts you write during the build."* **The pack ships zero executable verification.** |
| `js/fi-starter.js` | Real genre shells: web = `arcade`, `puzzle`, `neutral`, `narrative`, `sim` (~16KB each — fixed-timestep loop, input, WebAudio SFX, screenshake, and a `drawSprite()` that draws a colored box when art is missing so a build can never render nothing). Godot = one shell with `EventBus`/`GameState`/`AudioManager` autoloads. |
| `JUDGE_POLICY` (`export-pack.js:171`) | Tier 1 = 1 round advisory. Tier 2 = 3 rounds blocking, refs required. Tier 3 = 5 rounds blocking, refs required. |
| `FI-PLAYBOOK.md` | 47KB, 7-phase protocol, anti-reskin FAIL rule, demands a **measured** frame number in the declared worst-case scene, honest-failure path that drops the tier rather than certifying. |
| `.claude/skills/` | `fi-game-build`, `fi-graphics`, `fi-audio`, `fi-judge`, `fi-visual-overhaul`, `fi-design-lab`, `fi-playtest`. |
| Site | Plain HTML/CSS/JS, no build step, one Python 3.9 stdlib-only process (`tools/fi-dev-server.py`). Admin approve is a human click. |

### 2.1 The correction that matters most

**FI's shipped judge is already bounded, and the first draft of this plan was about to delete that.**

- `export-pack.js:48` — *"`${judge.rounds}` rounds is a cap this pack set, not a number you pick. **Exhaust it and you STOP**."* **VERIFIED.**
- `fi-judge/SKILL.md:70` — *"Loop until `OVERALL: PASS` **or until the stamped budget is spent, whichever comes first**."* **VERIFIED.**
- `SKILL.md:78` — plateau exit at two consecutive flat rounds. `SKILL.md:79` — budget spent → honest FAIL. **VERIFIED.**

The 261-run day came from an **unbudgeted** prose judge. FI already fixed that class of failure. **Do not remove the round budget.**

And critically: **Immortal Shores has no `FI-PLAYBOOK.md`, no `AGENTS.md`, no `.claude/` directory at all.** The canonical failure case *was not running the FI pack.*

### 2.2 The one-word defect

`refs: 'required'` at tiers 2 and 3 orders the judge to *"fetch and VIEW real screenshots of comparable commercial titles and compare them side by side with yours."*

That is **confound #2 — target-medium mismatch — shipped as policy.** A canvas/SVG game diffed against commercial screenshots is structurally unreachable, which is exactly the Immortal Shores failure (ten AI-generated *painted* concept boards stamped as the acceptance target for a procedurally-authored glTF kit). A gate pointed at an unreachable target says NO forever, correctly, and is a churn machine for a beginner.

`.claude/skills/fi-visual-overhaul/SKILL.md` carries an **independent copy** of the blocking gate and the required reference comparison. Any edit must sweep it too. **VERIFIED.**

---

## 3. The product: **FI Foundation**

> **The floor every FutureIndustries game starts on: it already looks deliberate before you write a word, and nothing in the pack points it at a target it cannot reach.**

**Named FI Foundation, not FI Harness.** The Gestalt store confirms `fi-harness` is already the orchestrator cockpit (Weyla). Two products, one name, one founder's inbox is a real cost. Per the standing ship-over-IP rule, the name does not gate the build — decide it once, at landing-copy time, and move on.

### 3.1 What it is not

- **Not a quality judge.** No score, no 1–10, nothing that rescales between rounds. `JUDGE-RUBRIC.md` and `fi-judge` survive — a human deciding what is good was always the part that worked, and admin approve stays one click.
- **Not a gate.** Nothing blocks export, submit, approval, or hosting. Gate volume is the binding constraint on a solo founder and this adds zero.
- **Not 84 rules.** The completeness critic's month-two prediction is the design constraint: *"Roughly twenty-five blocking gates… the agent is fenced out of the rubric; the human is not. By week six the population is running an unenforced harness, every pack looks green, and FI has no idea which gates were removed."*
- **Not a new repo, service, npm package, or build step.** No new process on the Ubuntu box.
- **Not an interview.** No locked Export button, no seven cards in front of a person who came here to skip the gauntlet.

### 3.2 The three pillars

#### Pillar 1 — Roles, not hex *(the strongest confound, cheapest fix)*

Every color in every shell becomes a semantic role (`ink`, `paper`, `ground`, `structure`, `accent`, `hazard`, `shadow`, `ui`) resolved through `LOOK.role()`. The creator never types a color — they point at one of ~24 hand-tuned, contrast-checked palettes rendered as **live moving tiles** in the wizard.

`LOOK.role()` must return a value usable as a canvas `fillStyle`, an **SVG `fill` attribute**, and a CSS custom property. *(Non-negotiable: CURFEW's `art.js` has zero canvas references and 43 inline SVG path literals. A canvas-only primitive set would be blind to FI's own best game.)*

**Why it earns its place:** the lever already exists and nobody pulled it — every shell routes color through `GAME.config.colors.a/b/c` in one `config.js`, and `E.drawSprite` already takes a color argument. **VERIFIED.** It reaches every creator on day one, not just the one genre with a finished painter pack. And it is a *removal*: the number of ways a beginner produces an ugly palette drops to zero because the operation is not offered.

#### Pillar 2 — Painters, not files

Art is a function of `(role, seed, sim state)`, shipped as plain JS in `web/looks/`. **Tier 1 stops shipping `assets/prompts/AI-PROMPTS.txt` and the empty `assets/sprites/` folder.** That folder is where art vintages breed — and the current export contradicts `FI-GRAPHICS.md` in the same zip that ships it. Tiers 2 and 3 keep a narrow singular-asset lane, quantized to the locked ramp on load.

**Evidence:** CURFEW's `art.js` is 13,337 bytes producing 29 distinct enemies from a small body/accessory vocabulary plus an archetype table. That grammar already shipped and is already loved.

**This pillar's real payload is recoverability (§0.4).** NileInSpace died because "the visuals are bad" meant rewriting a 5,973-line `station.js`. Under painters, the same sentence means editing one function and reloading. **A product that makes ugly recoverable is worth more than one that makes ugly detectable** — the owner could already see it was ugly.

**The named risk, stated up front:** FI now owns the art direction of every Arcade game, so the failure mode flips from *inconsistent* to *identical*. Do not defer this to milestone six — see §5, Q4.

#### Pillar 3 — Point the target at something reachable

`refs: 'required'` → `'optional'` **at all three tiers**, sweeping `fi-visual-overhaul/SKILL.md:49-51` and the published tier-card copy at `prompt-builder-wizard.js:65,73`. **Keep rounds 1/3/5. Keep the plateau exit. Keep the honest-FAIL path.**

When a creator types *"I want it to look like Hollow Knight,"* they get a card: *Hollow Knight is hand-painted 2D made by a studio; here is the closest thing your medium can actually reach* — rendering live, right there — and two buttons: **use this**, or **keep it as a mood note**. The reference the pack compares against is a contact sheet **the same renderer already drew**, so an unreachable target is not implementable.

> **Open correction from the adversarial pass:** a mood note must have a *destination* (palette suggestion, motif seeding — something), not be formally discarded. Naming games they love is the only design vocabulary a non-expert has, and it is FI's best signal about what they actually want.

#### Pillar 4 — The Look Board *(owner requirement, 2026-08-03)*

> **Eric:** *"we need some kind of asset review board, before any assets get wired in, the user should see them all on an artboard with an approve or deny with the user feedback in it… however, we don't want the builder to get stuck using that asset if perspective isn't quite right, we still want to build them from scratch to fit the environment… but we want to know what they will look like generally ahead of time."*

Every asset the pack will draw, shown on one board **before a line of game code is written**, so the look is settled up front instead of discovered at round 30. What is approved is **direction, not pixels** — the builder still draws each asset in place, fit to its own environment, scale and perspective.

This directly attacks the verified *criteria-authored-mid-flight* failure: Immortal Shores added acceptance criteria three separate times during the build (18:02 black rings, 20:03 outline/ghost buildings) and **the score dropped on every addition** — 8.0→6.6, then 7.6→7.1. The gate could never close because the gate was still being written.

**This has been attempted once and it failed. The evidence is in the repo.** `ART-STORYBOARD-ANSWERS.json` records ten boards approved between `23:13:17.978` and `23:13:30.605` — **12.6 seconds, ~1.26s per board, every one "yes," no per-item notes.** `ART-STORYBOARD-APPROVED.md` already stated *"Boards are inspiration only — not final meshes/sprites."* **VERIFIED.** Three failures, and the design must beat all three:

| What went wrong | What this design does differently |
|---|---|
| **Approval carried no information.** 1.26s/board is a rubber stamp; a yes/no can always be clicked. | **It is a chooser, not an approver.** Every item is pick-one-of-N variants (or a slider), and "approval" is the accumulated set of picks. You cannot advance without choosing, and a choice carries information even when made in a second. Picking is also *easier* for a non-expert than critiquing — it does not reinstate the expertise barrier. |
| **"Inspiration only" was doctrine in a doc, not a property of the mechanism.** The judge diffed against those boards for 30 rounds anyway. | **The board renders parameters, and only parameters are frozen.** The look contract stores palette, density, motif, silhouette class and per-item notes — never an image path. The downstream judge is structurally unable to receive an external reference image, because none exists in the pack. |
| **The boards were Grok Imagine JPEGs** — painted art in a medium the glTF kit could never reach. | **The board is drawn by the game's own renderer, on the game's own projection, at the chosen palette.** What you approve is reachable *by construction*. This is only possible because of Pillars 1–2: art is painters, so the board is a staged screenshot rather than a gallery. It also satisfies FI-GRAPHICS' existing rule — approve only from full-size in-game frames, never thumbnails or crops. |

**Make the notes primary and the verdict derived.** The one part of the 2026-08-02 attempt that carried real signal was the free-text *"Director notes (locked law)"* — money-shot POV, whole settlement visible, not a low mid-iso close-up. That is what shaped the build. The yes/no column shaped nothing. So per-item free text is the first-class input; the approve/deny state is computed from whether a choice was made.

**What "build from scratch to fit" means mechanically.** The board publishes a *vocabulary* (which painter, which silhouette class, which density, which palette roles) and the game calls that vocabulary in situ. A building drawn on the board in isolation and the same building drawn into an occupied tile at the game's camera angle are the same painter with different arguments — so there is nothing for the builder to get stuck on, and no image to match pixel-for-pixel.

**The open risk, stated plainly:** forced choice is a hypothesis about your own behaviour, not a proven fix. If the next board is cleared in 15 seconds with three picks and no notes, the mechanism has failed again and should be cut rather than reinforced. **Instrument it: record time-per-item and note-length on the first real run.** That is the cheapest possible test and it fits in the export.

### 3.3 Cut, and why

**The seeded-bot `FINISH.html` page and its blindness self-test are cut from v1.** They were the centerpiece of the synthesis and the feasibility pass killed them on verified grounds:

1. **The design contradicts itself.** `games/curfew/play/test.html` loads only `rng.js`, `data-*.js` and `engine.js` — it never loads `ui.js` (43,774 B), `art.js` (13,337 B), `fx.js`, `main.js`, or `style.css` (22,572 B). It is a headless **engine** driver, structurally blind to the entire render layer. A palette check needs the render layer loaded. You cannot claim both the 4-second/no-render shape *and* the palette check from one runner.
2. **It re-derives the APS failure one level up** and stamps it with authority — a page saying *"we proved these checks work"* about a game whose 22.5KB stylesheet could be deleted with nothing going red.
3. **`UNKNOWN` fires hardest exactly when the creator is most original**, and the shipped skill would make repairing FI's introspection harness the agent's top priority — spending a beginner's session on FI's tooling instead of their game.
4. **The agent cannot own the criterion that ends its own session.** Its cheapest repair for "nobody ever won in eight runs" is always *loosen the bot*, never *make the win reachable*.

**What survives from it, downgraded and worth doing:** ship each shell's placeholder game with a CURFEW-shaped `test.html` **already green** (rng + engine + a seeded `botRun` over five seeds), and tell the agent to keep it green — **never as a stop rule.** ~150 lines per shell. It forces the agent to build a headless-drivable core, which is the actual architectural payload of CURFEW and the thing that made it finishable, and it carries none of the ownership conflict.

**Termination comes from a budget, not a predicate.** Keep `judge.rounds`. Add one published sentence: *N build passes, then hand back; everything else goes in `NEXT-TIME.md`.* A budget terminates unconditionally. A predicate terminates only if it is reachable.

---

## 4. Build sequence

Each milestone is independently shippable. **M1 alone is worth doing even if nothing else is built.**

| # | Goal | Exit criterion | Size |
|---|---|---|---|
| **M1** | **Point the target at something reachable.** `refs:'required'` → `'optional'` at all three tiers. Sweep `fi-visual-overhaul/SKILL.md:49-51`, `prompt-builder-wizard.js:65,73`, `JUDGE-RUBRIC.md`, `TIER-REQUIREMENTS.md`, `pack-kit/design-lab/README.md`, and the two generated bundles via `tools/embed-*.py`. Fix `export-pack.js:382`, which calls fi-judge *"blocking (all tiers)"* while tier 1 is advisory. **Keep rounds 1/3/5, the plateau exit, and the honest-FAIL path.** | **Behavioral, not a grep.** Build the same one-liner twice, pre- and post-change; compare rounds spent and wall-clock to hand-back. n=2 is weak but it is evidence rather than a string search. | 2–4 days |
| **M2** | **Roles and a look you point at, across every shell.** Semantic roles replace `colors.a/b/c` in all five canvas shells and the DOM shell. `js/look-palettes.js` ships ~24 ramps. `LOOK.sprite` intercepts `E.drawSprite` and quantizes to the locked ramp. The wizard's style step becomes the Look Picker. Translation card handles typed commercial references. | (1) Grep gate exits non-zero on any hex literal under `starter-templates/web/` outside the palette module. (2) Picker renders ≥12 live tiles; exported pack opens in the picked palette **by double-click from `file://` on Chrome, Safari and Firefox**, zero `fetch` calls. (3) A named commercial title produces the Translation card. (4) `LOOK.role()` drives the same palette correctly as canvas `fillStyle`, **SVG `fill` attribute**, and CSS custom property. | ~1 week |
| **M3** | **Kill the vintage farm.** Drop `assets/prompts/AI-PROMPTS.txt` and the empty `assets/sprites/` from tier-1 packs. Ship `NEXT-TIME.md`. Publish the pass budget and a wall-clock hand-back in `SKILL.md` / `CLAUDE.md` / `AGENTS.md` / `CURSOR-RULES.md`. | A tier-1 pack contains zero image files and zero prompt files; a tier-2 pack still contains both. The pass budget appears in all four agent-facing files. | 2–3 days |
| **M3b** | **The Look Board** (Pillar 4). Every painter the pack will draw, rendered by the game's own renderer at the chosen palette, as pick-one-of-N per item with free-text notes primary. Freezes parameters into `look-contract.js` — never an image path. Instruments time-per-item and note-length. | A board of ≥8 items is completed, and the recorded median time-per-item is **>8s** with **≥3 items carrying notes**. Below that it is a rubber stamp — the 2026-08-02 result — and the mechanism is cut rather than reinforced. | ~1 week |
| **M4** | **The real test.** One fresh pack, one agent, one sitting. Eric opens **no image editor** and writes **no prose judgement**. The pack reaches a hand-back and is submitted to the Arcade. | Recorded honestly whether or not it works. **This is the only outcome that matters** — everything above is a hypothesis until this runs. | 1 sitting |
| **M5** | *Conditional.* Seeded `test.html` per shell, green on arrival, agent told to keep it green. Never a stop rule. | Each shell ships a green `test.html` that goes red when the placeholder mechanic is removed without teaching the bot the new verb. | ~1 week |
| **M6** | *Conditional, gated on §5 Q4.* Second look pack. | Only if the identical-catalog question has been answered. | — |

---

## 5. Owner decisions — **ANSWERED 2026-08-03**

Source: `FI-FOUNDATION-REVIEW-ANSWERS.md`, exported from the review page.

| Item | Ruling |
|---|---|
| Pillar 1 · Roles not hex | **BUILD** |
| Pillar 2 · Painters not files | **BUILD** |
| Pillar 3 · Reachable targets | **BUILD** |
| Pillar 4 · The Look Board | **BUILD** |
| M1 · Reachable-target fix | **GO** |
| M2 · Roles + Look Picker | **GO** |
| M3 · Kill the vintage farm | **GO** |
| M3b · The Look Board | **GO** |
| M4 · The real test | **GO** |
| M5 · Seeded `test.html` | **HOLD** |
| Q1 · Name | **FI Foundation** |
| Q2 · Tier-1 image lane | **REMOVE** at tier 1, keep at tiers 2–3 |
| Q3 · Build-pass budget | **3 build passes + 90-minute hand-back** |
| Q4 · Identical catalog | **Set the variance bar UP FRONT**, not at M6 |
| Q5 · Worth the weeks | **PARK** |

### 5.1 Resolved: ratified and parked

Q5 = PARK read against M1–M4 = GO. The owner confirmed the intended meaning: **the milestone answers ratify *which* work is right; Q5 answers *when*, and the answer is not yet.** The design is approved as the plan and shelved. FIMemory comes first, per the standing strategy that the income wedge is FIMemory and games are supporting proof.

**Do not start building any milestone in this document.** Pick it up when Arcade supply becomes the binding constraint.

**The one cost this knowingly accepts.** M1 is 2–4 days and removes a defect FI ships **today**: every tier-2 and tier-3 pack instructs the agent to fetch real screenshots of commercial titles and diff them against a canvas or SVG game (`refs: 'required'` in `js/export-pack.js`, plus an independent copy in `.claude/skills/fi-visual-overhaul/SKILL.md:49-51`). That is the Immortal Shores failure encoded as policy, and it keeps going out while parked. Recorded here so it is a decision that was made, not one that was overlooked — **it is the first thing to do when this unparks.**

---

### 5.2 The original questions and reasoning (retained for the record)

These are yours. Research cannot settle them and the design branches on each.

**Q1 — The name.** `fi-harness` is already the orchestrator cockpit in the store.
→ *Recommend:* **FI Foundation** for this; leave the cockpit alone (it has a build log, a memory topic, and a dogfood thread under its current name). Do not spend a day on it.

**Q2 — Does tier 1 really stop shipping `AI-PROMPTS.txt` and `assets/sprites/`?**
→ *Recommend:* **remove at tier 1, keep at tiers 2 and 3.** A toggle is worse than either option — it puts the expertise decision back on the beginner. Tier 2 *is* the toggle, and it already exists.

**Q3 — What replaces the round budget as the published stop rule?** M1 keeps `judge.rounds`, but the pack has never published a *build-pass* budget.
→ *Recommend:* name two numbers before M3 — a pass budget (e.g. 3 build passes at tier 1) and a wall-clock hand-back ("past 90 minutes, stop and hand back"). Unconditional, zero new code.

**Q4 — The identical-catalog question, decided now rather than at M6.** If FI owns the art direction, the catalog stops being inconsistent and starts being *identical*. The structural answer is that a look pack is a parts vocabulary plus a recombination table (CURFEW gets 29 enemies from ~5 bodies and ~20 accessories) and variation compounds across seed × palette × density × motif — but that is an argument, not a proof.
→ *Recommend:* decide the acceptable variance bar **before** five games ship from one pack, because FI cannot un-ship them.

**Q5 — Is this the right thing to spend weeks on at all?** The standing strategy says the income wedge is FIMemory and games are supporting proof. FI Foundation is supply-side work.
→ *Recommend:* **M1 regardless** — it is 2–4 days and it removes an unreachable target FI is actively shipping. M2–M4 only if Arcade supply is the current bottleneck. This is a real strategic call, not a formality.

---

## 6. Honest gaps

Named, not hidden.

- **Cost model.** Nobody priced this. The unit is not the run, it is the **agent turn** the run triggers — billed to the creator's metered Claude Code or Cursor subscription. *"Needs: Nothing. A browser and your AI. Costs money: No"* is true about **FI's** bill and silently false about the **creator's**. FI's pack design decides how much of someone else's quota a build consumes and has never measured a single turn.
- **`FINISH`-style verification is deferred, not solved.** The render-layer-vs-headless contradiction (§3.3) is a real unresolved design question. Decide it *in writing* before any future attempt.
- **Godot track gets nothing** in v1. Named as not-done rather than shipped half-done.
- **The Arcade genre vocabulary still doesn't match the shells.** `fi-dev-server.py:102` speaks `colony, trade, async, survivor, roguelike, map, comedy, satire, sim`; the shells speak `arcade, puzzle, sim, narrative, neutral`. Unresolved. Note that `pickWebShell` currently routes `/deck/` to the **arcade twitch shell**.
- **Content safety and rating.** The completeness critic flagged this as a platform-ending gap: `fi-dev-server.py` carries `"rating": None` and `_validate_arcade_metadata` never validates a rating field, on a public arcade with no content classifier. Out of scope here; do not let it stay out of scope everywhere.
- **Blocked-run resolution.** No rollback-to-last-green, no scope downgrade, no escalation path. Every hard failure is a support ticket in one inbox.
- **Immortal Shores is not frozen.** The Gestalt store has entries from **2026-08-03 12:11 / 12:40 / 13:21** describing `author_kits.py`, coplanar-face fixes, and "all 15 kinds re-exported." None of it is on this PC — HEAD is `eedc37f` at 08-02 20:15 and `author_kits.py` does not exist here. **VERIFIED.** That is live cross-machine divergence and the "canonical failure" framing in the prior docs is stale.
- **One forensic claim could not be confirmed.** An agent reported the store logging a PASS for commit `1eee257`. That commit is genuinely not in the repo (**VERIFIED**), but the store entry was not found either. Recorded as unconfirmed rather than repeated.

---

## 7. Provenance

| Swarm | Agents | Output |
|---|---|---|
| A — corpus synthesis + FI grounding | 16 | 561 raw claims → 84 canon rules, 38 cut, 26 echo warnings, 43 do-not-canonize, 7 owner decisions, completeness critique |
| B — variance forensics on 14 project records | 8 | Gate hypothesis **refuted**; termination reframe; 9 alternative causes ranked |
| C — four competing architectures, 3 judges, synthesis, feasibility attack | 9 | This design, with its centerpiece cut by its own feasibility pass |

Every claim marked **VERIFIED** was checked by the author against disk or the live web. The claims that survived that check are the ones this document is built on.
