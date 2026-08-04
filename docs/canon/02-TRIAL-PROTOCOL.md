# The trial — notes for the weekend game

- **Status:** Notes. Not started. Run when you choose to.
- **Purpose:** Administer the treatment that has never once been administered, and get one number that decides everything downstream.
- **Companion:** `01-BUILD-PLAN.md`

---

## Why this exists

**Verified across six repos: zero `.github` directories, zero non-sample git hooks.** No CI, no pre-commit, no wired runners — including in FI-Harness itself.

The gate hypothesis is **refuted as an explanation** of your past outcomes (P(good | strong gate) = 1/5 vs 2/3) and **untested as a prescription.** There has never been a treated group. Everything in the 84-rule canon is downstream of an assumption nobody has checked.

One weekend checks it.

**And one thing verified on disk that removes the last excuse:** every NileInSpace sim module — `state.js`, `production.js`, `settlers.js`, `zones.js`, `trade.js`, even `station.js` — loads in bare node with no DOM. Only `main.js` needs `document`. CURFEW's browser-global sim layer loads under `node:vm` with a three-line fake `localStorage`, runs seeded games to completion, exits 0, zero npm deps. Node v24.18.0 and git 2.55.0 are on your PATH right now.

**A headless bot harness was always an afternoon's work. It was never written.** That is the administration problem, and it is the thing being tested.

---

## The game

**One CURFEW-shaped web game. New theme.** Discrete turns, one screen, fully seeded, ~3,000–4,000 lines, 15–25 content entities, one win condition, one loss condition, **zero external asset files.** Hard-capped at **3 working sessions.**

**Explicitly not:** a colony/economy sim, anything 3D or Godot, anything with exteriors or a world map, anything multiplayer or server-backed, anything real-time or continuous.

**The theme is the only free variable. The shape is fixed.**

Three reasons, in weight order:

1. **Confound control.** If you test the prescription on a new genre at a new scale, a bad result tells you nothing — you won't know whether the gates failed or the scope did. Hold the shape constant at the one that already worked and vary only the treatment.
2. **Discrete + seeded is what makes the instrument possible.** A bot can play a turn-based seeded game to win-or-dead, which gives a machine-checkable "is this finishable" signal for free. `curfew/play/test.html` already does this in 422 lines. Real-time or 3D destroys that signal and leaves you eyeballing it again. **The genre choice is the instrumentation choice.**
3. **It attacks the actual failure mode.** A weekend-sized game can reach done *before the abandonment window opens.* Scope isn't a nice-to-have here — it's the primary intervention.

---

## Phase 0 — set the trap before the game exists

**Hard cap: 2 hours.** Blow the cap → ship the trap as-is and start the game anyway.

1. **One sentence in `README.md`.** Discrete turns, one screen, seeded. If the sentence needs an "and", cut until it doesn't.
2. **`ACCEPT.md` — 5–8 criteria, each a yes/no a stranger could check.** Include the win condition, the loss condition, and *"a bot completes a seeded run to win-or-dead."* Commit it. **This file is FROZEN — you may not edit it after the first line of game code.** Your scores dropped every time criteria moved (8.0→6.6, 7.6→7.1).
3. **Run the Look Board now**, before any game code. Every visual element the game will draw, on one page, pick-one-of-N. **Rules: a typed note on every row, and the board is not approved in under 10 minutes.** Ten boards rubber-stamped in 12.6 seconds is a click-through, not a review. You are approving **direction, not pixels.** Commit as `LOOKBOARD.html`.
4. **Build four tools in `tools/`. Hard cap 400 lines total.** Do not make them good. Make them exist.
5. **Wire `.git/hooks/pre-commit`. Then deliberately break something and watch a commit get REFUSED.** Do not proceed until you have seen it with your own eyes. **This single step is the entire treatment, and it is the step that has always been silently skipped.**
6. **Create `METRICS.tsv`, log `t0`.** Everything is measured from here.

### Runnability notes — verified on this box today

- git 2.55.0 runs `.git/hooks/pre-commit` under its **bundled bash**, so the hook must be a **POSIX sh file with a shebang**.
- Make the hook call **`node tools/gate.mjs`**, not PowerShell. Node resolves inside that bash; an execution-policy prompt would **silently un-administer the treatment.**
- **No `||` fallback anywhere in the hook.** `cmd || echo ok` reports success when the command is missing — that pattern bit twice on 8/1.

---

## The four scripts

| Script | Does | Generalized from | ~Lines |
|---|---|---|---|
| `tools/sim-check.mjs` | Imports sim modules directly in node. N named assertions + 5 full seeded bot runs to win-or-dead. Asserts **same-seed-same-outcome AND different-seed-different-outcome.** `exit(fail>0?1:0)`. | `curfew/play/test.html:298-402` (`botRun`/`botFight`), `2342/godot/tests/logic_check.gd` (50 seeded trials, `quit(1)`), `Immortal-Shores/tools/critical-audit.mjs` | ~150 |
| `tools/art-check.mjs` | Every entity the sim can produce has a registered painter; painters deterministic for fixed (role, seed, state); no hex literals; output non-degenerate. **Prints `FLOOR CHECK — does not measure whether it looks good` on every run.** | `NileInSpace/tools/aaa-judge.py`, **inverted** — same reasonable checks, honest label. The banner is the whole point. | ~80 |
| `tools/mutation-guard.mjs` | Copies the repo to temp, blanks the stylesheet and the painter module, re-runs everything, **exits 1 if the suite still passes.** | The one-off APS mutation experiment (deleting `style.css`, gutting `index.html`, throwing in `ui.js` → still 92/92 exit 0). Make it standing so the result can't silently rot. | ~60 |
| `tools/gate.mjs` + hook | Runs sim-check and art-check, one-line verdict, non-zero on failure. Hook refuses the commit. | **Generalized from the ABSENCE.** Nothing to copy — it has never once existed in your work. That is precisely why it is the experiment. | ~40 |

Plus `tools/log.ps1` (~15 lines) appending a timestamped row to `METRICS.tsv`.

**The different-seeds-differ check is not optional.** A probe run today produced a bot that returned byte-identical results across different seeds because it never actually made a decision. **A determinism check alone will happily pass on a degenerate bot.**

---

## Phase 1 — sim to first playable *(target: under 6 hours)*

7. **Write the simulation first, as plain ESM modules with ZERO DOM references.** No `document`, `window`, `canvas` or `requestAnimationFrame` in the sim layer. Not architecture astronautics — it is the single constraint that keeps `sim-check.mjs` runnable, and it costs nothing from line one.
8. **Add assertions as you write each rule, not after.** Implement a status effect → write the assertion pinning its number **in the same commit.** CURFEW pins exact damage values (6, 9, 12, 15, 24) — that is why it has grip.
9. **No file over 900 lines, none over one third of the codebase.** Measured on your own work: CURFEW 31.6% (885), 2342 13.7% (382), **NileInSpace 45% (5,748)** — the one that went terminal is the outlier. When a file crosses 900, split it **that day.** Do not finish the feature first.
10. **First playable = bot completes a full seeded run to win-or-dead, same seed twice gives same outcome, two different seeds give different outcomes.** Log TTFP.

## Phase 2 — art, under budget *(target: 3 rounds, 5 hard)*

11. **Art is painter functions of (role, seed, state).** Semantic roles only — never a typed hex literal in a painter. **Zero image files, zero audio files, zero .glb.** Both your coherent outcomes shipped exactly this.
12. **Timer on at the start of each art round, off at the end. Log human-minutes-per-art-iteration.** If HMAI crosses 10 minutes you have re-introduced a hand-driven pipeline — **stop and find it, don't push through.**
13. **Judge art against `LOOKBOARD.html`, never against commercial screenshots.** Reference images optional at every tier. An unreachable target is why `aaa-judge.py` drifted into measuring frame time and cyan pixels.
14. **Keep the round budget. 5 rounds max on visuals. Exhaust it and STOP and ship what you have.** This is the one thing in the existing canon that is verified sound.
15. **Visual acceptance is you, once, looking at a screenshot next to the Look Board, saying yes or no.** No script decides this. **Nothing you can automate measures whether it looks good** — a check counting art files with solid alpha and a money-shot that isn't black is a floor check, and labelling it a ship gate is what produced a false PASS 6/6.

## Phase 3 — prove the suite has teeth, then finish

16. **Run `mutation-guard.mjs`. Do not ship until it reports grip.**
17. **Ship it to the FI Arcade.** Not "ship-ready," not "READY-TO-DEPLOY" — **live, in the catalog, playable by a stranger.** This is the only outcome that counts as finishing.
18. **Fill in the last row of `METRICS.tsv` and read the seven numbers.** That is the result.

---

## Standing rules — these are the ones that break your pattern

19. **NO NEW PROJECT FOR 14 DAYS.** No new repo, no new game folder, no "quick prototype." Your abandonment signature is **6 hours** from one project going dark to the next being created, and **110 minutes** from a last judge run to a full rewrite. **The new project is not a fresh start. It is the failure.** If you stall, write one line in `METRICS.tsv` saying what stalled you and stop for the day.
20. **NEVER TOUCH THE TOOLS AFTER PHASE 0.** If a gate is wrong mid-build, note it in `METRICS.tsv` and keep building. **Fixing the harness during the build is how the harness becomes the project.**
21. **Commit every time the gate goes green.** More than 4 hours with no commit is the abandonment window opening — stop and commit whatever works.
22. **When stuck on the look, re-render, don't rewrite.** If you are about to rewrite the renderer, art has leaked into the sim layer — **go find the leak.** That leak cost 6,000 lines last time.

---

## The seven numbers

| Metric | Healthy | Note |
|---|---|---|
| **Gate-block count** | **3 or more** | **THE MOST IMPORTANT NUMBER. Check it on day one, not at the end.** 0 means the treatment was not administered — again — and the experiment is void regardless of how the game turned out. |
| Time-to-first-playable | under 6 h | Over 12 h means the scope is wrong. Cut entities immediately, don't push on. |
| Rounds-to-visual-acceptance | ≤3 | Primary outcome measure for painters. 5 is the hard budget. |
| Human-minutes-per-art-iteration | under 10 | **Proves or disproves recoverability.** Past 20 you have rebuilt the hand-driven pipeline. |
| Longest silence gap | under 24 h | Your abandonment tell. Over 48 h → ship whatever passes the gate. |
| Suite grip | PASS | Mutation guard must *fail* on the mutant. |
| Concentration | under 35%, hard ceiling 900 lines | CURFEW 31.6% · 2342 13.7% · NileInSpace 45%. |

---

## Pre-registered outcomes — sign these before Phase 0

**CONFIRMED** — live on the Arcade within 3 sessions, gate-block ≥3, RTVA ≤3, mutation guard reports grip.
→ One clean administered trial beats 84 unadministered rules. Extract the generic `tools/` **from what worked** into the export step.

**VOID** — gate-block count is **0**, no matter how well the game went.
→ The hook never bound: either it wasn't wired or the gates sat below where the work lands. **Do not read a smooth build with a zero here as a win.** Re-run with tighter assertions.

**FALSIFIED** — bot-verified playable, gates green, mutation guard has grip, RTVA inside budget — **and it's still unshipped 72 hours later.**
→ Machine enforcement and painters don't touch your actual failure mode. You don't stop because you can't verify or because art is unrecoverable, but for reasons upstream of both. **If that happens, stop building harnesses.** The next intervention is about commitment and finishing, and every further hour on gates is treating the wrong disease. **The entire 84-rule canon is downstream of the assumption that verification is the lever.**

**SECONDARY FALSIFICATION** — RTVA hits 5, or median HMAI exceeds 20 minutes.
→ Painters did not deliver recoverability; art is still hand-driven under a new name. Pull that pillar back out of the ratified design and reconsider it **independently** — painters and the bot harness are orthogonal, so one can fail while the other succeeds.

---

## The biggest risk

**The harness becomes the project.**

This is by far the most likely way it goes wrong, and it is the exact failure the research effort already exhibited at larger scale: 561 claims, 33 agents, 84 canon rules, a full design spine — **and zero git hooks, and a `tools/` directory the exporter emits empty.** Enormous investment in the apparatus of rigor, none of it ever wired to anything.

**The specific shape it will take:** you start writing `sim-check.mjs`, notice it would be more useful if it were generic across games, notice that generic version is basically the studio's M0, and by Sunday you have a promising harness and no game. **It will feel like progress the whole time, because it is legitimately useful work.** That is what makes it hard to see from the inside.

**The guard is the Phase 0 cap and rule 20.** Four small scripts, 400 lines, two hours, then frozen. **Write them badly on purpose.** Hard-code paths. Don't parameterize. The generic version gets written *after* a game has shipped, extracted from something that worked — never before, never during.

**Do not treat this build as the studio's M0 while you are running it.** Everything here will feed that later. But if you build it as a product you will make it good, and making it good is what will consume the weekend.

> **The experiment is the deliverable. The game is the evidence. The harness is scaffolding you are allowed to throw away.**
