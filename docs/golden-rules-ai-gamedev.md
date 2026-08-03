# Golden Rules for AI-Assisted Game Dev

**Collected:** early August 2026  
**Window:** ~June – early August 2026  
**Sources:** High-engagement Reddit (r/aigamedev, r/gamedev, r/IndieDev, r/godot, related) and X/Twitter; weighted toward ship posts, postmortems, and patterns repeated across independent sources.

Examples: *Cook the Dungeon* (Steam + Cursor/Godot), Godot MCP vs CLI debates, *CLAUDE.md for game devs*, daily AI tooling threads on r/gamedev, failed metroidvania postmortem, agent workflow posts (plan → implement → self-check).

---

## 1. Building the Harness / Agent Tooling / Orchestration

### 1. Encode project law in a living root file (`CLAUDE.md` / `AGENTS.md` / `.cursorrules`), not in chat memory

**Why it works / failure it prevents:** Every new session is a brilliant hire with amnesia. Without architecture, conventions, and “what not to do,” the agent invents a parallel codebase.

**Tools / techniques:** Claude Code `CLAUDE.md`, Cursor rules, `AGENTS.md`.

### 2. Prefer plan → human approve → implement → self-verify over “write the code”

**Why it works / failure it prevents:** Agents that study structure first and check their own work (tests, headless run, screenshots) ship; agents that dump code need babysitting.

**Tools / techniques:** Boris Cherny–style loop: architecture/git awareness → plan → change → verify.

### 3. Use MCP surgically; prefer files + CLI for bulk authoring (especially Godot)

**Why it works / failure it prevents:** Fat MCP tool schemas burn tokens and slow loops. Headless Godot + text `.gd`/`.tscn` often outperforms “use the MCP for everything.” Reserve MCP for live editor state, surgical scene edits, running-game inspect, screenshots.

**Tools / techniques:** Godot CLI/headless, lean MCP (batch tools / exclude domains), custom debug TCP bridge into the running game.

### 4. Ground the agent in truth sources (engine source, local docs, skills)—not only web docs

**Why it works / failure it prevents:** Docs lag; grepping a matching engine tag (e.g. `4.x-stable`) answers “what actually happens” in one step.

**Tools / techniques:** Clone engine **outside** the project; path in `CLAUDE.md`. Optional: docs/changelogs as local markdown.

### 5. Keep harness thin: few agents, few tools, explicit policies

**Why it works / failure it prevents:** Multi-agent “studio” cosplay often wastes context. Shippers repeatedly use 1–2 agents with clear rules.

**Tools / techniques:** Skills for repeated workflows; one custom CLI tool > 40 vague MCP tools; policy docs the harness can enforce.

### 6. Add mechanical gates the model cannot talk past (tests, contracts, ship-check)

**Why it works / failure it prevents:** “Looks right” is not shippable. Subagent review passes (debug debris, unknown deps, broken surfaces) catch AI residue.

**Tools / techniques:** GdUnit / unit tests, ECS input–output contracts, `/ship-check`-style multi-pass reviews.

---

## 2. Creating Sufficient Game Details & Specs so an AI Can Build Reliably

### 1. Spec the game rules that force architecture—not a novel-length GDD

**Why it works / failure it prevents:** Economy as integers, turn order, fog rules, combat formula, etc. stop the model from inventing floats, wrong update order, and soft “defaults.”

**Template sections:** Overview, stack, architecture, conventions, design-that-affects-code, common tasks, anti-patterns.

### 2. Maintain living markdown as the real memory: ARCHITECTURE, FEATURES, TECH STACK, TODO, CHANGELOG

**Why it works / failure it prevents:** Compaction and handoffs lose chat history; git-backed docs survive. Update them every time the design or structure moves.

**Pattern:** Strong consensus in daily AI gamedev threads.

### 3. Define acceptance criteria and success tests *before* implementation

**Why it works / failure it prevents:** Without ACs, agents “complete” tasks with shortcuts (fake state machines, empty modules, silent fallbacks).

**Pattern:** TDD / tests-first + “verifiable goals from ACs.”

### 4. Prefer data-driven design (JSON/resources for balance, content, buildings, cards)

**Why it works / failure it prevents:** Forces parameters you can tune without re-prompting systems; agents hardcode less.

**Godot-friendly:** `.tres` / `res://data/*.json` called out in rules.

### 5. Write “Common Tasks” as checklists (add building, add enemy, add card)

**Why it works / failure it prevents:** Turns vague “add X” into the same file layout, test, and UI hook every time—reduces architecture drift.

### 6. Scope to something an agent *can* finish: Act 1, vertical slice, or genre with clear systems

**Why it works / failure it prevents:** Full metroidvanias / large coupled systems collapse; deckbuilders, autobattlers, single-file arcade, and HTML/browser games ship more often.

**Failure mode:** Non-coder “product owner only” on complex 60fps chaos without learning the engine.

### 7. Put anti-patterns in writing from past failures

**Why it works / failure it prevents:** “No game state in nodes,” “no await in `_ready`,” “fail loud not default,” etc. prevent re-breaking the same things.

---

## 3. Visual (and Audio) Asset Creation + Review

### 1. Build a *style lock* (moodboard + fixed style prompt + reference images), not freeform chat art

**Why it works / failure it prevents:** Chat threads drift; ship-quality AI games use a custom or fixed pipeline for consistency (e.g. Cook the Dungeon’s ChatGPT Images tool + moodboard).

### 2. Generate multi-view / multi-pose sheets in one shot when possible

**Why it works / failure it prevents:** Same-generation sheets keep identity more consistent than sequential “now side view” chats.

### 3. Human still owns the last mile: crop, UI polish, micro decisions, Photoshop/Figma

**Why it works / failure it prevents:** Raw gens are not production assets; polish is what makes “not AI slop.”

**Pipeline examples:** ChatGPT → Meshy → Unity; Meshy + Suno; ElevenLabs + Suno for VO/music.

### 4. Review art and audio *in game*, not only in the generator gallery

**Why it works / failure it prevents:** Feedback on trailers repeatedly hits “generic SFX,” busy cameras, inconsistent UI—even when stills look fine.

### 5. Prefer coherent, constrained art directions over max photoreal

**Why it works / failure it prevents:** Stylized / low-poly / pixel / card art masks gen variance better; photoreal multiplies identity failures.

### 6. Treat audio as intentional craft, not a one-click bed

**Why it works / failure it prevents:** Shippers get praise for music/VO but critique for stock-feeling SFX; align hits, UI, and music to the same design language.

### 7. Disclose AI asset use where store policies require it; don’t assume “coding only” is free of rules

**Why it works / failure it prevents:** Steam/store and community backlash still shape discoverability and trust.

---

## 4. Agentic Loops, Iteration Rhythm, and Success Criteria

### 1. You are director + QA; the agent is implementer—not the creative lead

**Why it works / failure it prevents:** Agents code features fast but cannot judge fun, cohesion, or “should this exist.”

**X pattern:** Talk like a game director with references, screenshots, and examples.

### 2. One vertical slice that plays, then expand—never one-prompt full game

**Why it works / failure it prevents:** MCP maintainers and shippers agree: precision tools + iteration win; one-shots produce pretty demos or broken piles.

### 3. Close every loop with evidence: run, test, screenshot, or play path—not “looks good in code”

**Why it works / failure it prevents:** Self-seeing agents (tests / screenshots / headless boot) fix themselves; blind agents invent green builds.

**Godot tip:** Headless boot to catch startup crashes and generate UIDs.

### 4. Keep iterations small and review each one

**Why it works / failure it prevents:** Multi-constraint tasks (standards *and* memory *and* net *and* feature) thrash; small PR-sized chunks survive review.

### 5. Byte-identical rollback before risky changes; instrument when performance dies

**Why it works / failure it prevents:** Silent regressions and recursive audio/perf bugs are common; ship stories include days lost without instrumentation.

### 6. Success criteria = player-facing: playable Act 1, Steam page + trailer, external playtest—not “agent said done”

**Why it works / failure it prevents:** Wishlist + demo feedback beats internal “vibes.” Cook the Dungeon pattern: Act 1 done → share → later demo.

### 7. Kill scope when litmus tests fail (adversarial review, fake FSMs, unmaintainable scripts)

**Why it works / failure it prevents:** Honest postmortems beat endless token spend on unshipable complexity.

---

## 5. Implementation & Code Workflow Best Practices

### 1. Demand surgical changes; ban speculative “while we’re here” refactors

**Why it works / failure it prevents:** Adjacent “improvements” break unrelated systems and erase working feel.

### 2. Prefer fail-loud over defensive defaults

**Why it works / failure it prevents:** AI loves `|| default` and silent catch; that hides missing data until balance/debug is impossible. Rule: crash early is a feature.

### 3. Keep systems modular, API-first, and swappable

**Why it works / failure it prevents:** Agents replace parts more reliably than they redesign coupled spaghetti; treat features like library modules.

### 4. Git is the memory; don’t let the agent own history carelessly

**Why it works / failure it prevents:** Docs + commits + branches beat chat recall; many pros still gate agent git operations.

### 5. Match engine strengths to AI: Godot text scenes excel; graphics/shaders/feel need extra human taste

**Why it works / failure it prevents:** Models struggle more with niche graphics, multithreading, and “feel”; harness them for boilerplate, tools, UI, systems, docs.

### 6. Review like a senior hiring a junior: always read diffs

**Why it works / failure it prevents:** Working-but-wrong code is the failure mode; skills atrophy if you never check.

### 7. Fight architecture drift continuously: correct → add rule → re-run

**Why it works / failure it prevents:** Even with rules files, code drifts; living RULES/CLAUDE.md is a feedback log of mistakes.

### 8. Budget tokens deliberately (workhorse models for bulk, heavy models for hard design)

**Why it works / failure it prevents:** MCP-everything and multi-model thrash can cost $1k/month; Codex/Sol/CLI loops often 2–10× cheaper for comparable bulk work.

---

## 6. Hosting, Publishing, and Marketing

### 1. Ship a Steam (or itch) page + trailer as soon as a slice looks real

**Why it works / failure it prevents:** Wishlist is the leading indicator; polished trailers with sound convert better than “AI tech demos.”

**Example:** Cook the Dungeon store page + trailer during WIP Act 1.

### 2. Market the game, not the pipeline

**Why it works / failure it prevents:** Audiences care about fun; over-centering “100% vibe coded” invites AI-slop discourse and platform/community friction.

### 3. Put a demo in front of real players before full content

**Why it works / failure it prevents:** External feedback (camera shake, SFX, mobile fit) is faster than more generation.

### 4. Build in public with short clips and specific asks (“wishlist,” “playtest Act 1”)

**Why it works / failure it prevents:** High-engagement posts that show playable video outperform abstract “how do I use AI?” threads.

### 5. Match distribution to format: browser/PWA/HTML for tiny loops; Steam for commercial roguelikes; mobile when UI/session length fit

**Why it works / failure it prevents:** Single-file Claude games and Steam Godot titles both ship—pick the channel that matches scope.

### 6. Keep launch honest on AI use and polish bar

**Why it works / failure it prevents:** Disclosure, consistent art, and non-generic audio reduce ban/reputation risk and “slop” dismissal.

### 7. Use early wishlist traffic as design input, not only marketing vanity

**Why it works / failure it prevents:** Shippers iterate camera, UI, and feel from comments within hours of posting.

---

## Top 5 overall meta-rules (across the pipeline)

1. **Harness > hype** — Living project law + verification beats “better prompts” and multi-agent cosplay.
2. **Director, not typist** — You own design, fun, scope, and review; AI accelerates implementation.
3. **Evidence every loop** — Tests, headless runs, screenshots, play—green narratives without proof don’t ship.
4. **Small slices, real ships** — Act 1 / demo / Steam page beats unfinished “full game” dreams (and failed metroidvania-scale vibe projects).
5. **Consistency is craft** — Style locks for art, modular APIs for code, git-backed specs for memory—drift is the default failure mode of AI gamedev.

---

## Quick snapshot: what’s working in mid-2026

| Layer | Often works | Often fails |
|-------|-------------|-------------|
| Code | Cursor / Claude Code / Codex + Godot text, plan mode, rules files | One-shot full games, no review, no engine literacy |
| Tooling | CLI/headless + lean MCP + skills | MCP-for-everything token burn |
| Art | Moodboard tools, multi-pose sheets, Meshy pipelines + human polish | Unlocked chat gens, photoreal identity chaos |
| Ship | Vertical slice → trailer → wishlist → demo | Endless prototype / “AI made a GTA” marketing |

---

## Source anchors (non-exhaustive)

- r/aigamedev: *Cook the Dungeon* ship/share post (Cursor + Godot + custom ChatGPT Images art tool; Steam page)
- r/aigamedev: MCP vs no-MCP Godot experiment + maintainer guidance (CLI for bulk; MCP for live editor)
- r/aigamedev: Clone Godot engine source + path in CLAUDE.md
- r/gamedev: “AI coding tools daily—what works” (living docs, fail-loud, harnesses, small modules)
- Godot forum / r/gamedev: vibe-coded metroidvania failure postmortem (scope + literacy limits)
- mrphilgames.com: *CLAUDE.md for Game Devs* template (architecture, design context, anti-patterns)
- X: agent ship gap (code vs ship); plan → verify loops; director-style prompting with Unity MCP

---

*Saved to FI-Harness for harness design reference. Expand into engine-specific `AGENTS.md` templates as needed.*
