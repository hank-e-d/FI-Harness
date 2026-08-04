# Build plan — one product, one bench, one trial

- **Status:** Plan only. **Nothing is being built.** Owner instruction 2026-08-03: *"just get the plan ready and the notes. We aren't building yet."*
- **Supersedes:** the two-product split as proposed. See §1 — it did not survive its own stress test.
- **Companion:** `02-TRIAL-PROTOCOL.md` (the notes — the weekend trial you actually run)

---

## 1. What happened to the two-product split

The owner approved *"upgrade the build pack, build the new studio, then throw a game at it."* A six-agent pass then defined the studio, allocated all 84 canon rules across the two products, designed the ladder, and ran a dedicated skeptic against the whole thing.

**The skeptic won, and the synthesis conceded.** Three findings, all checkable:

**The allocation refutes the split arithmetically.** Of 84 rules: **PACK 1, STUDIO 34, BOTH 46, NEITHER 3.** Exactly one rule lands purely in the shipped product. The split doesn't reduce scope, it *relocates* it — 34 rules into a product that doesn't exist and 46 into a maintenance obligation that now exists twice, in two enforcement languages, kept in sync by one person with no test that fails when they drift.

**The timing is the tell, and it's about my behaviour in this session.** `6f68f25` ratified the narrow design at **20:48:51**. `fa9f388` parked it for capacity at **20:50:34**. **Ninety-nine seconds later** the response was a nine-command CLI, eight kernel modules, two CI surfaces and an M0–M5 roadmap the proposal itself priced at *"months for one person."* The parked thing was the small thing. That is a parking decision being routed around.

**The founding premise was factually wrong, and this is the amendment that saves most of the value.** I built the boundary on *"the pack is browser-only, no terminal, so it structurally cannot ship a runner."* That conflates the human with the agent. **The standing no-terminal rule constrains what the creator types, not what their agent executes.** Every pack user is by definition running Claude Code or Cursor locally — they have a shell, and they have Node, or the agent could not write files.

So `node tools/sim-check.mjs`, an exit code the agent reads and acts on, a determinism check, a seeded headless bot, a mutation probe, and a pre-commit hook written by the export step are **all pack-shippable today**, into the `tools/` directory `js/export-pack.js:1179` currently emits empty. Six of the ten "impossible in the pack" capabilities collapse.

**Three survive honestly:** Playwright's Chromium download (a real install), memory that spans games, and the Godot track. *Three capabilities is not a product line.*

---

## 2. The ruling

> **One creator-facing product** (the FI Build Pack, with depth as a dial inside it).
> **One private instrument** (an unnamed bench — Eric only, disposable, explicitly not a SKU).
> **FIMemory unchanged** as the only thing anyone pays for.

The split is **not killed — it is gated.** It becomes a decision the evidence makes, and the evidence costs one weekend.

### The boundary rule

> **Anything the creator's own agent can execute inside the exported folder with zero installs ships in the pack — exit codes and the pre-commit hook included. Only what needs an install, memory that spans games, or a paid generator stays on the bench.**

### What this preserves of the owner's intent

| Owner said | Status |
|---|---|
| "upgrade the build pack" | **Yes — and further than before.** `tools/` ships populated, not empty. |
| "build the new studio" | **Yes as a bench, no as a product.** You get the tooling; it doesn't get a name, a ladder, a price or a support inbox. |
| "throw a game at it and test" | **Yes — this is now the centerpiece, and it comes first.** |

---

## 3. Concessions, recorded

- **Scope relocation. Conceded.** 1/34/46/3 settles it.
- **"Cannot ship a runner" is false. Conceded — the load-bearing error.**
- **R3/R4 (ship a seam you never use; freeze field names across a repo boundary) are a tax on the shipped product for an unbuilt one. Dropped.** The seam still ships, but because it is good structure for the pack's own agent — not as a subsidy to a hypothetical.
- **The nine-item `pack_must_not` veto list is dropped.** Feature-freezing the working product to preserve rungs for an unbuilt one is a real cost in roadmap options. If pack users ask for session resume or genre packs, they get them.
- **Graduation-by-withheld-enforcement is crippleware by omission. Conceded.** A designed exit whose trigger is *"my AI keeps breaking the thing that was already good"* — when the platform has the fix and declines to ship it — violates the proposal's own first rule. Free does not launder it.
- **The old boundary gave the pack the half that already failed. Conceded.** "Pack gets the seeing, studio gets the blocking" hands strangers *a human reading a page and judging* — precisely the mechanism that produced a false PASS 6/6 holding four iterations.
- **The Weyla precedent lands.** Power-user tooling, founder as user zero, strong design spine, a 7/20 dogfood pass, unmerged branches, gotchas logged as recently as 8/2, no revenue, still open. **Two power-user tools for user zero is a habit, not a portfolio.**
- **"The harness becomes the project" is self-indicting. Conceded.** One half of the proposal named it the dominant risk and prescribed 400 disposable lines frozen after two hours; the other half specified the product-grade generic version of those same four scripts. They cannot both ship. **The disposable half ships.**

**Not conceded:** `docs/00-07` stay. The skeptic is right that `06-roadmap.md` M3 makes the spine an internal *factory*, not a SKU — but the genre catalog, mechanics fence, extension map and lane model are real content the pack needs at Depth 3. **Relabel in place; do not archive.**

---

## 4. Two corrections to the record

**The name "FI Harness" is free.** `00-SYNTHESIS` §3 asserted it was taken by the orchestrator cockpit. **The cockpit was renamed Weyla on 2026-07-20.** The recommendation not to spend time on naming stands, but the stated reason was wrong.

**NileInSpace's sim was always testable, and this reframes the whole diagnosis.** Verified: `state.js`, `production.js`, `settlers.js`, `zones.js`, `trade.js` **and** `station.js` all load in bare node with no DOM. Only `main.js` needs `document`. **A seeded headless bot harness was an afternoon of work at any point in that project's life. Nobody wrote one.**

That is the cleanest confirmation of the headline finding: **the blocker was never architectural. You do not have a capability problem. You have an administration problem.**

It also splits the diagnosis in two. `station.js` is a **render** monolith sitting on a **perfectly testable sim**. So *painters* (recoverability) and *the bot harness* (finish signal) are **orthogonal interventions and must be measured separately.** The trial protocol does that.

---

## 5. Sequence

### Do now — this week

| # | Action | Cost | Why now |
|---|---|---|---|
| 1 | **Pre-register the falsification terms in writing**, before any code. | 20 min | Criteria authored mid-flight is a documented failure here — Immortal Shores added them three times and the score fell every time (8.0→6.6, 7.6→7.1). Sign the terms first or the result gets reinterpreted after. |
| 2 | **Run the trial** (`02-TRIAL-PROTOCOL.md`). | 1 weekend, 3 sessions hard cap | Needs no product built, does not unpark FIMemory, produces Arcade supply either way, and is the only thing that converts the gate hypothesis from *untested* to *tested*. |
| 3 | **Relabel `docs/00-07` in place** as the depth-and-factory spine, on a branch. One README paragraph: this is the internal build system that produces packs (`06-roadmap.md` M3 already says exactly this), not a second SKU. | 30 min | Those docs currently read as a second product's spine. That reading is what produced this detour. |
| 4 | **Add the demand instrument** — one line in the pack's `NEXT-TIME.md` template inviting a creator to write in if they want to keep going past one evening. | 15 min | The "~10 unprompted requests" trigger can never fire without it. **Zero replies is itself a decisive answer.** Cheapest datum in the plan. |

### Gated on the trial's numbers

| # | Action | Gate |
|---|---|---|
| 5 | **Extract the generic `tools/` from what worked** into `js/export-pack.js`, so the zip stops shipping an empty directory. Days, not months. | **Only if the trial CONFIRMS.** Generalizing an unadministered treatment is how this detour started. |
| 6 | **Run the extracted `tools/` unchanged against `curfew/play/`.** | If they run without a rewrite, the tooling generalizes. **If they only run on games the tooling created, the depth story is decorative.** |

### Explicitly NOT started

`bin/fih.mjs` and the nine-command CLI · `kernel/{registry,runner,dump,frame,perf,mutate,ledger,report}.mjs` · the versioned check registry · per-pack `AGENTS.md`+`STYLE-CONTRACT.md`+six subdirectories · genre packs · M0–M5 · `.github/workflows/harness.yml` as a product surface · R1–R8 · the veto list · the ladder and graduation copy · **any product name, any trademark screening, any price or seat.**

Every one is downstream of a number that does not exist yet.

### Revisit the split only when these fire

- FIMemory's first charge landed, or **D2 formally closed**
- The trial **confirmed**
- A creator **who is not Eric** has *finished and submitted* a pack game
- ~10 unprompted "how do I keep going" requests in a quarter
- Product A support load measured on a real cohort and near zero
- **A second maintainer, or an economic owner who is not Eric**

Two products, two doc sets, two rot surfaces and one inbox is the configuration that makes this a mistake. Change the maintainer variable and most of the objection dissolves.

---

## 6. Open owner decisions

1. **The no-terminal rule — authoritative reading.** Does *"every terminal action becomes a UI element"* constrain **the human only**, or **the agent too**? The entire single-product resolution rests on the first reading. **If you rule the second way, the pack cannot ship teeth and the split argument gets significantly stronger.** Only you can rule on this.
2. **Does "shipped" mean live in the catalog, or played by a stranger?** Define it before Phase 0 — the 72-hour falsification clock hangs off it.
3. **Pre-commit the VOID rule.** If gate-block count comes in at **0**, do you re-run with tighter assertions, or accept that verification isn't the lever for you and redirect to finishing? **Decide now, in writing** — this is the only decision that cannot be made honestly after the fact.
4. **Depth as a dial — confirmed?** Accepting it means dropping the veto list and letting the pack grow session resume and genre packs when users ask.
5. **The bench stays unnamed.** Recommended, given Gestalt→Squirl→FIMemory, Nexus→Canopy→Weyla, and registration 5027436 killing Squirl. If you want a name anyway, say so and accept it as a fourth screening bill on a product with no revenue.
6. **D2 is the only thing that unparks anything.** A non-code item gating the first card charge — which makes it the most displaceable task on the board, **which is exactly why a second product looked appealing 99 seconds after parking the first.**
7. **The theme of the weekend game.** The only free variable in the protocol. Pick it before Friday so Phase 0 doesn't spend its two-hour cap deciding what the game is about.
8. **Weyla.** Same shape as the proposed studio, still open, unmerged branches, gotchas as recent as 8/2. Finished, formally parked with a written stop, or archived? **Leaving it ambiguous is what makes a third power-user tool feel like a fresh start rather than a repeat.**
