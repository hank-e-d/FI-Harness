# Trial log — what the first administered run produced

- **Status:** Session 1 complete, **paused**. Game playable, not shipped.
- **Ran:** 2026-08-06 23:09Z → 2026-08-07 ~01:00Z, one sitting.
- **Repo:** `C:/Users/eric_/GameMaking/eighteen` — **local only, no remote.** Its own `HANDOFF.md` carries resume instructions.
- **Governs:** `02-TRIAL-PROTOCOL.md`, `03-PREREGISTRATION.md`

> **The findings below are the deliverable. The game is the vehicle.** This file is the part that survives whether or not `eighteen` ever ships.

---

## 1. The treatment was administered

For the first time across seven repos, **a machine refused a commit.**

```
BLOCKED: ACCEPT.md CHANGED after freeze.
    locked 63f74dfa5fe3a82b  now 5483bdb21092b898
>>> git exit code: 1
```

**gate-block: 2.** Both fired on the frozen-contract check, and the second one matters more than the first:

- **Block 1 — the drill.** Criterion A7 removed from a frozen `ACCEPT.md`. Refused.
- **Block 2 — unplanned, and the useful one.** `git checkout -- ACCEPT.md` restored the file from the **index**, where the bad copy was already staged. The hash still mismatched and the gate refused again. **It caught a real mistake made while undoing a fake one.**

### The single most transferable finding

**Both blocks came from the contract check. Not one code check ever fired.**

Give the gate something to guard **before there is any code to check**. A hash-pinned acceptance file has teeth from minute one, costs about twenty lines, and enforces exactly the rule Immortal Shores broke three times while its score fell 8.0→6.6 then 7.6→7.1.

---

## 2. The numbers

| Metric | Value | Healthy | |
|---|---|---|---|
| gate-block | **2** | ≥3 | short by one |
| time-to-first-playable | **0.25 h** | <6 | |
| rounds-to-visual-acceptance | **2** | ≤3 | |
| human-min-per-art-iteration | 28.4 (board) | <10 | one-off setup, not per-iteration |
| longest silence | 1.55 h | <24 | |
| suite grip | **PASS** | must PASS | all 3 mutants caught |
| concentration | **21.1%**, largest file 359 lines | <35%, <900 | NileInSpace was 45% in one 5,748-line file |

**A gap in the pre-registered terms, flagged rather than papered over:** at gate-block 2 with the game shipped, the result is not CONFIRMED (needs 3), not VOID (needs 0), not FALSIFIED (it shipped). I left an undefined middle. The honest reading is that a third block is required, and it should be **earned during real work rather than drilled**.

---

## 3. For the build pack — ship these

A working reference implementation exists in `eighteen/tools/`, **287 lines across five scripts**, zero dependencies, zero installs. Per ruling R1 the agent runs them and the creator never types a command.

| Ship | Why |
|---|---|
| **A hash-pinned `ACCEPT.md` + freeze check** | The only thing that actually bit. ~20 lines. |
| **Seeded bot to win-or-dead** | Generalized from `curfew/play/test.html`. Answers the one question a machine can honestly answer: is this finishable. |
| **`different-seeds-differ`, not just determinism** | **Essential.** A determinism check alone passes happily on a bot that never decides — a probe during planning produced exactly that, returning byte-identical results across every seed. |
| **`art-check` printing `FLOOR CHECK — this does NOT measure whether it looks good`** | This is `NileInSpace/tools/aaa-judge.py` inverted. Same class of checks, honest label. That mislabelling produced a false PASS 6/6 that held four iterations. |
| **`mutation-guard`** | Makes the one-off APS forensics a standing script. Reported GRIP here. |
| **A POSIX-sh pre-commit hook, no `\|\|` fallback** | `cmd \|\| echo ok` reports success when the command is missing. |
| **The inline-modules build pattern** | Chrome blocks ESM over `file://`. Every browser pack hits this. Generate the HTML from source; node still imports the real `.mjs`, so one source of truth. |

**Painters-not-files held up.** Concentration 21.1% against NileInSpace's 45%. Every visual change this session was a function edit and a reload — never a rewrite.

---

## 4. For Pillar 4 — the Look Board went through four revisions

**Every defect was found by a human noticing something felt wrong. No gate caught any of them.** Five rules, each learned the expensive way:

1. **Options must differ STRUCTURALLY, never by seed.** Rev 1 offered four seeds of one painter — a few units of jitter. Owner: *"all of these look the same, im having a hard time making meaningful choice based on something tangible."*
2. **Offer a drawing-style axis, not just colour.** Rev 1 had none, so every option differed only in hue. Treatment (flat/inked/hatched/blocked) is a bigger lever than palette and was entirely absent.
3. **Never ask a global property once per item.** Rev 2 asked framing eight times, which manufactured variance and then recorded it as preference. Owner: *"answering that i like it wide every single time made me doubt i was doing it correctly since why would i need to say wide over and over."* **The board corrupted its own data, and I logged the corruption as his taste before he corrected me.**
4. **Do not build an expertise gate.** Rev 3 asked for framing, treatment, density and palette as separate axes — 288 combinations in vocabulary a non-expert does not have. Owner: *"i just think people will be lost... we are at a level of detail that people wont understand."* **It had drifted from the ratified Pillar 1 wording — "two clicks, no hex codes, no art words, nothing to be wrong about" — and I did not notice.** Rev 4 collapsed it to eight named complete looks, one question, expert axes behind a disclosure.
5. **Flag a palette+treatment pairing below a contrast floor.** `papyrus` had `structure` vs `ground` at **1.06:1** — a building and the earth under it at identical luminance — and `blocked` carries no outline. The board let him choose a combination that physically cannot read.

### Board fatigue is real, measurable, and a minimum-duration gate does not catch it

Seconds per row: **225 / 179 / 108 / 83 / 66 / 56 / 36 / 16.** Monotonic, **14.1×**. Note quality tracked it exactly — rows 1–2 gave real direction, rows 5–8 gave *"not sure"*, *"hard to make out"*, *"kind of obscure"*, *"i cant make out what some of these last ones are"*. The 10-minute floor caught a rubber stamp and missed a fade.

**The board DID work once fixed:** 28.4 minutes against 12.6 seconds on 2026-08-02, with a 237-character actionable defect note. **The notes carried everything; the picks carried almost nothing.**

---

## 5. The finding that limits what the harness may claim

**Nothing in the harness tests whether a tool can be used by the person it is for.**

`art-check` proves painters exist and are deterministic. `sim-check` proves the game finishes. `mutation-guard` proves the suite can see the game. All three were green through every single board defect above.

**Every usability failure in this session was found by a human.** The harness has no instrument for usability and should stop implying it does. This is a hard limit on the FI Foundation pitch, not a gap to be closed with another gate.

---

## 6. Open

- **`eighteen` has no remote.** Local on one disk, against the standing rule. Decide: its own repo, or copy `play/` into `FutureIndustries/games/eighteen/` at ship time (the path CURFEW took).
- **One more gate-block, earned not drilled.**
- **Ship it live** — `"shipped"` = live in the catalog (R4).
- Then, **and only if the trial confirms**, extract `tools/` into `js/export-pack.js` **from the thing that worked** — so the zip stops emitting `tools/` as an empty directory at line 1179.
