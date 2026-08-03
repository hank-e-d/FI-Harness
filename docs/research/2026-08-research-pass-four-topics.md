# Research pass: four AI game-dev workflow topics (Jun–early Aug 2026)

- **Compiled:** 2026-08-03  
- **Window:** ~June – early August 2026  
- **Method:** High-signal Reddit (r/aigamedev primary; r/LocalLLaMA, r/IndieDev, r/AI_Agents, r/mcp related) + X; prioritize demos, measurements, postmortems  
- **Author:** Independent research pass (this agent)  

This note is the **campaign report** for four research prompts. Deep dives live in linked files (including notes already in-repo from prior agents).

| Prompt topic | Primary digest(s) |
|--------------|-------------------|
| Harnesses, loops, eval, long-run failures | [Harness golden rules](2026-08-agent-harness-golden-rules.md) · [Workflows consensus](2026-08-agent-workflows-emerging-consensus.md) · **§1 below (fresh findings)** |
| Specs / GDD / context engineering | [Specs agents can build from](2026-08-specs-agents-can-build-from.md) · **§2 below** |
| Visual & audio assets | **[Assets digests](2026-08-visual-audio-asset-pipelines.md)** (new) |
| Hosting, publishing, marketing | **[Ship & market digests](2026-08-hosting-publishing-marketing.md)** (new) |

**Meta-thesis across all four topics:**

> Shippers win by **constraining generation** — of code, of scope, of pixels, and of distribution ambition — then iterating with **external truth** (playtests, metrics, humans, store rules).

---

## §1 Harnesses, agentic loops, eval, failure modes

### Fresh golden rules (this pass)

#### H1. Prefer task-gated loops over 24/7 autonomy

**Rule:** After each subphase, a human (or independent checker) must play the game before the next task starts.

**Why:** Fully autonomous multi-week loops produce “headless walking corpses” — tests may pass while gameplay is nonsense. r/aigamedev (late Jul 2026): OP ran agents 24/7 for 2 weeks; community consensus was *quality not speed*, playtest after every task, not every few days.

**Patterns:** Spec-driven design doc + dependency roadmap; subphases sized to one model session; checkpoints between subphases; adversarial QA agent with clean context (`/loop` + separate reviewer).

#### H2. Systems, not features — and “learn” skills after sessions

**Rule:** Ask for systems (weapon/event/notification managers); after hard-won fixes, run a **learn** skill that writes lessons into a knowledge file referenced by `CLAUDE.md`/`AGENTS.md`.

**Why:** One-shot features thrash; re-solving the same bug burns tokens. Repeated on “how are you making good games quickly?” thread.

**Tools:** Headless Godot (often preferred over Godot MCP for speed); skills for map/combat/cutscenes; multi-model leads (Opus plan / Sonnet implement — with caveats that cheap workers can cost more in fixes).

#### H3. Tests are backpressure, not proof of fun

**Rule:** Automated tests prevent regressions; they do not replace play. Acceptance criteria must be **observable gameplay**, not “audit sections green.”

**Why:** 27-systems postmortem (self-graded pass @ ~3 FPS). Tool success ≠ outcome success (r/AI_Agents).

#### H4. Harness instrumentation for games is numbers + state JSON

**Rule:** Before content, ship telemetry, scenario runners, golden frames, and structured game-state dumps agents can read.

**Why:** GTA-like Gauntlet Loop demos: JSON game state ≫ video alone; Playwright gameplay assertions; real GPU for perf truth.

*(Full harness rules, MCP skills split, external bounds: see linked digests.)*

### Failure modes (reinforced this pass)

| Mode | Avoidance |
|------|-----------|
| Autonomy without judgment | Human play gates; adversarial checker |
| MCP tax | Headless CLI first; MCP only for editor/scene needs |
| Context accretion / compaction dumbness | Sub-agents; durable STATE.md; learn files |
| One-shot AAA myth | Ignore engagement bait; polish still costs human time |

---

## §2 Specs / details / context engineering

### Fresh golden rules (this pass)

#### S1. Design doc + roadmap → session-sized subphases

**Rule:** Planning model produces (1) systems design and (2) dependency-ordered roadmap where each subphase fits one coding session; coding agent updates both after work.

**Evidence:** Top practical reply on r/aigamedev “good games quickly” — same pattern as Day 31 design-brain vs implementation-agent split.

#### S2. Interview the human; never let the model interview itself for vision

**Rule:** Creative “why / heart / loop” comes from human answers; AI deepens questions, then freezes GDD. AI must not invent the fantasy unattended.

**Evidence:** Long constructive comment same thread — unattended AI produces AAA clones and perfect UI without a fun loop.

#### S3. Precision where rules exist; placeholders where hypothesis is mechanical

**Rule:** Numbers, states, win/lose, APIs in-scope; placeholders OK until the prototype question is answered.

*(Full 11-rule specs digest already in-repo.)*

---

## §3 Visual & audio (summary → full file)

See **[2026-08-visual-audio-asset-pipelines.md](2026-08-visual-audio-asset-pipelines.md)**.

Headline rules: lock style (LoRA / style bible / palette); separate art pipeline from code agents; engine-ready contracts (size, sheet, pivot); review in-game not in gallery; audio via dedicated generators + placeholder SFX packs; never let the coding agent invent final art programmatically as default.

---

## §4 Hosting, publishing, marketing (summary → full file)

See **[2026-08-hosting-publishing-marketing.md](2026-08-hosting-publishing-marketing.md)**.

Headline rules: itch = players/feedback, Steam = customers when polished; disclose AI per platform rules; short vertical video > essays; wishlists before launch; don’t flood stores with half-demos (platform backlash risk); sell the fantasy and fun, not “made with AI.”

---

## Cross-topic synthesis matrix

| Concern | Harness | Specs | Assets | Ship |
|---------|---------|-------|--------|------|
| Source of truth | Repo + STATE | ACCEPTANCE > MILESTONE | Style bible + asset contracts | Store checklist + trailer |
| Loop unit | One subphase | One context packet | One character/sheet batch | One platform milestone |
| Success signal | Tests + play | Observable ACs | In-engine consistency | Wishlists, playtime, reviews |
| Stop condition | Green AC + human | Non-goals | Pass QA rubric | Ready for customers |
| Common trap | 24/7 autonomy | Giant GDD | Style drift / AI slop look | Steam before polish |

---

## Confidence & caveats

- **High** on harness lean-context, systems-first, maker≠checker, vertical slice, itch-vs-Steam hassle, AI disclosure culture.  
- **Medium** on specific product rankings (Pi vs OpenCode vs Hermes; Spriterrific/AutoSprite/Capybara-class tools — fast churn).  
- X threads incomplete; weight multi-source Reddit postmortems higher.  
- Another agent’s [independent crosscheck](2026-08-golden-rules-independent-crosscheck.md) may overlap; prefer additive links over merge wars.
