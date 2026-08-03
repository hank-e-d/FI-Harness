# AI Game-Dev Golden Rules: Verified-Source Pass (Jun–early Aug 2026)

**Related in this repo:** a second independent pass at this brief, run from Eric's own four prompts verbatim (harness/loops/eval/failure-modes; specs & context engineering; visual/audio assets; hosting/publishing/marketing). Complements [docs/golden-rules-ai-gamedev.md](../golden-rules-ai-gamedev.md), the agent-harness digests, and [2026-08-golden-rules-independent-crosscheck.md](2026-08-golden-rules-independent-crosscheck.md) — added unmerged per repo convention.

**Sourcing note — read before citing:** reddit.com and x.com/twitter.com were both confirmed blocked to direct fetch in this run's environment too (reddit: hard "unable to fetch" / 403 on mirrors; x.com: HTTP 402 Payment Required on every attempt). Unlike the earlier crosscheck pass, though, this run pulled a meaningful amount of **directly-fetched, independently verifiable primary material**: several Hacker News threads fetched live via the HN Algolia API with real point/comment counts (e.g. Godogen's Show HN at 337pt/205c, a parallel-Claude-agents-via-tmux thread at 189pt/131c, an AI-playtesting thread at 136pt/36c), direct devlogs (dev.to/yurukusa, itch.io devlogs, roboticape.com, mrphilgames.com), and direct GitHub repos (godot-agent-loop, godogen, gstack-game, Claude-Code-Game-Studios, ai-game-art-pipeline-skill). Where a claim traces only to press coverage *quoting* a Reddit/X post (e.g. the Steam Next Fest disclosure backlash, Matt Shumer's "Gauntlet Loop"), it's marked "secondary" inline. Treat directly-fetched sources as stronger evidence than secondary/press-quoted ones — each section flags which is which and lists real URLs at the end.

One of the four sub-passes also queried Gestalt (unlocked this session) and cross-checked against this user's own FI doctrine (`fi-graphics-doctrine`, `hd2d-art-pipeline`, `fi-brand-message`) — noted inline where it corroborates or extends existing FI patterns, logged to Gestalt as `ai-game-shipping-golden-rules` for reuse across FI Arcade titles.

---

# AI Game-Dev Agent Harnesses & Agentic Loops: Golden Rules

**Methodology note:** `reddit.com` and `x.com`/`twitter.com` are both blocked to direct fetch in this environment — Reddit returned an explicit "unable to fetch" error and a `redlib` mirror 403'd; `x.com` returned HTTP 402 Payment Required on every attempt, including direct search URLs. So no claim below is a screenshot of a live Reddit or X page. Relied on: (1) **Hacker News threads fetched directly**, quotable with real usernames; (2) **dev.to devlogs, GitHub READMEs, and Medium posts** by people who shipped actual prototypes, fetched directly; (3) **press/secondary coverage** (Decrypt, BigGo Finance, aggregator blogs) that quotes and links specific viral X posts (e.g., Matt Shumer's), not opened directly. Every rule is tagged with its evidence type. r/aigamedev itself is confirmed to exist (~55k members, 480% YoY growth, per subreddit-analytics sites) but its actual post threads weren't readable — only third-party summaries of its topic mix.

### 1. Separate the builder from the judge — never let an agent grade its own work
**Why it works:** An agent that wrote the code has already committed to its own justifications; asking it to also verify the result just re-confirms its own bias. A fresh-context critic that only sees the *artifact* (not the reasoning trail) catches gaps the builder is structurally blind to.
**Evidence:** The single most repeated idea across independent sources. Matt Shumer's viral "Gauntlet Loop" (X, ~3.8M views per secondary coverage in Decrypt/BigGo/thepromptindex/somethingbig.ai) is built entirely on this: builder and "ruthless blind critic" sub-agents with separate context, critic mandate = "only pass if better than a real-world equivalent." Independently, the yurukusa 5-agent-studio devlog (dev.to, direct) concludes "agents should never be the final judge on visual design, but they're great at producing 20 candidates fast for a human to pick from." The Digital Thought Disruption eval-harness piece (direct) formalizes this as three separated evaluation surfaces (outcome / trajectory / controls) rather than one self-report.

### 2. Ground success in the running artifact, not in "it compiled" or "tests passed"
**Why it works:** Compilation and unit tests check syntax and isolated logic, not the actual play experience — an agent can pass every green check and still ship something unplayable.
**Evidence:** yurukusa's 30-day Claude Max ledger (dev.to, direct) names this explicitly as the "signature pitfall": "The tests passed. The code compiled. The game launched. And the game was unplayable" (zero damage for 60 seconds; level-ups every 3.9 seconds against a designed 10–30s fun window). Godogen (GitHub, direct fetch of README + secondary press) solves it with a screenshot→Gemini-Flash-vision→fix loop that judges the *rendered frame of the live game*, not the build log. HN commenter **andai** on the "Ask HN: What are you working on" thread (direct) makes the general-case argument: models "can't actually play pong to see if they broke it." Godot Agent Loop's (GitHub, direct) whole design is the cycle **author → validate → run → observe → playtest → verify → refine** — "run" and "observe" are non-negotiable steps between "validate" and "verify."

### 3. Give the harness an inspectable, real reference bar — never a vague adjective
**Why it works:** "Make it AAA quality" is unfalsifiable; "match this Call of Duty screenshot" is a concrete comparison the critic sub-agent can actually check pixel-by-pixel.
**Evidence:** The specific mechanism Shumer's Gauntlet Loop credits for its result (secondary coverage, Decrypt/BigGo): three paragraphs, no renderer or system spec, just "match [named real game], utterly perfect," with the critic literally diffing against reference footage. Caveat (same Decrypt piece): since Three.js FPS patterns are heavily represented in training data, some of the "surprise" may be recombination rather than novel reasoning — discount proportionally when applying this to less-represented stacks (GDScript, Bevy).

### 4. Treat context as a depleting resource: isolate per-subagent, compact on trigger, re-anchor periodically
**Why it works:** Every model measurably degrades as effective context fills — benchmarked (Chroma's context-rot study across 18 models, cited secondarily: 95%→60-70% accuracy from short to long input at fixed task difficulty; a 4,416-trial study found constraint-compliance drops 73%→33% from turn 5 to turn 16 without mitigation).
**Evidence:** Chier Hu's Medium survey (direct) puts a number on it for coding agents: sessions "degrade after ~40 minutes as agents lose track of edited files," with large repos hitting a wall around 30k lines where grep returns swamps of false positives. Mitigations repeated across sources: subagent delegation with fresh, scoped context (yurukusa's studio: each agent owns an independent window; Shumer's critic gets a *clean* context specifically so it can't inherit the builder's rationalizations); `.claudeignore`/directory-scoped sessions; periodic re-anchoring. The awesome-harness-engineering list (GitHub, direct) catalogs tooling: `headroom` (60–95% tool-output compression), Token Savior (77% reduction via symbol-indexed nav), Claude Code's own progressive compaction.

### 5. Spend the scaffolding budget before blaming the model — CLAUDE.md, Skills, and engine-specific API docs
**Why it works:** Most "the model is bad at X" failures are actually "the harness never told it X." Frontier models hallucinate APIs for thin-training-data stacks (GDScript, Bevy) but stop once the real API surface is injected.
**Evidence:** Chier Hu's survey quotes indie dev "Mr. Phil" (Stellar Throne project): "every minute you spend on CLAUDE.md saves ten minutes of correcting AI-generated code," proposing a benchmark — **if you're hand-correcting more than ~30% of generated output, the scaffolding is underbuilt, not the model.** Godogen (GitHub, direct) bundles docs for 850+ Godot classes specifically to stop GDScript hallucination. Bevy devs report the model is "nearly always wrong" on its fast-moving API without explicit grounding docs pinned to a version.

### 6. Decompose into an explicit dependency graph, and tier models to task risk/cost
**Why it works:** Undeclared dependencies between parallel agents cause silent coordination failures; running every agent on the frontier model burns budget on trivial work.
**Evidence:** yurukusa's "5-Agent Game Studio" devlog (dev.to, direct): explicit `blockedBy` fields per task, agent self-assignment off a shared task list, asymmetric model routing — Opus for orchestrator/irreversible calls, Sonnet for implementation, Haiku for metrics/summaries — claimed **3–5x cost reduction** with no quality loss on cheap tasks. Same devlog: **5 agents is roughly the human oversight ceiling**; teams fanning out past 10 lose control. sorceress.games' model-choice piece (direct) generalizes this into Planner+Executor: expensive model plans/reviews diffs, cheap model does the ~90%-of-tokens typing — ~5x cost reduction, provided you never flip it.

### 7. Wire the loop into the engine itself — headless execution or a runtime socket, not "read the scene file and guess"
**Why it works:** An agent that can only read source files is blind to runtime state; one wired into a live, running instance catches physics/timing bugs no static read would surface.
**Evidence:** Godot Agent Loop (GitHub, direct) includes a "running-game socket" for live manipulation plus windowless-process scene authoring. Chier Hu's survey: headless-capable engines (Godot CLI, Pygame, browser-via-Playwright) let the loop close autonomously; GUI-bound workflows (Unity Editor, Unreal Blueprints) force human mediation. On the HN "Letting AI play my game" thread (direct, real usernames), **fishtoaster** found screenshot-only playtesting missed fast state changes, fixed only by combining a live code-level API with a browser MCP; **zoetaka38** reports swapping raw DOM dumps for accessibility-tree refs cut token cost ~10x per call.

### 8. Enforce completion in the harness, not in the prompt
**Why it works:** "Please double-check your work" is a suggestion a model can rationalize past; a harness gate that structurally can't be marked done without producing artifact X is not.
**Evidence:** Digital Thought Disruption's eval-harness piece (direct): authorization for tool use "should be enforced by application logic, not prompt instructions." Godot Agent Loop implements this literally: privileged tool groups denied by default, a human "Pause Agent" lock stops mutation during interactive editing.

### 9. Scope narrowly and validate mechanic-by-mechanic — never "build the whole game in one prompt"
**Why it works:** Compounding errors across a large ungrounded generation are far more expensive to untangle than catching one broken mechanic immediately after it's built.
**Evidence:** Summer Engine's blog (direct): "build one mechanic, run it, confirm it works, then add the next" — "any tool that claims [AI can build a whole game in one shot] is selling the demo, not the workflow," pegging realistic AI coverage at roughly the first 70% of a project. yurukusa's devlog independently names "scope collapse" as a top failure mode. Chier Hu's survey: roguelikes/puzzle/arcade-clones/turn-based-strategy are high-success categories; 3D AAA-scale, physics-heavy, asset-driven-workflow projects are high-friction.

### 10. Build a real eval harness: production contract → 3 evaluation surfaces → isolated trials → numeric release gates
**Why it works:** Vague "seems to work" testing doesn't scale and doesn't catch regressions; a harness with a written contract, reset state between trials, and calibrated numeric gates is the only thing that catches drift as the agent (or model) changes.
**Evidence:** Digital Thought Disruption's eval-harness piece (direct, dated 2026-07-31): write the "production contract" before tests; evaluate **outcome**, **trajectory**, and **controls** separately; reset all state between trials (most common practical mistake: reused state); start with 20–50 cases; recalibrate any model-based grader when the judge model changes; gate releases on explicit numbers (example: ≥98% regression pass rate, ≥97% required-tool recall, 100% critical-handoff accuracy, P95 latency/cost caps). Every production failure converts into a new regression case, safety case, corrected grader, or documented reason it was missed.

### 11. Keep the human as final judge of fun/taste — agents generate candidates, not verdicts
**Why it works:** Nothing in these harnesses evaluates subjective delight; agents also don't model the player's cognitive load, so tutorial/UX polish specifically needs direct human involvement.
**Evidence:** ksaun, describing the "Vestiges" roguelite build on an HN thread (direct), reports Claude rarely produces something that outright doesn't work, but "it's not good at imagining the player's perspective; it doesn't consider the player's cognitive load for UX considerations." Summer Engine's blog: "AI does the building. Scope, fun, and taste are still yours." yurukusa's devlog: use agents to fan out design candidates fast, but the human always makes the final pick.

**Cross-cutting pattern:** the builder/critic split (Rule 1) + grounding on the live running artifact instead of a compile/test signal (Rule 2) shows up, independently invented, in Shumer's Gauntlet Loop, Godogen's screenshot-vision loop, yurukusa's own "tests pass but unplayable" lesson, Godot Agent Loop's run→observe→verify cycle, and the Digital Thought Disruption eval-harness contract — five unrelated sources, same conclusion, same six-week window.

**Primary sources fetched directly:** news.ycombinator.com/item?id=47947525 ("Letting AI play my game"), id=48884984 (Ask HN, July 2026), dev.to/yurukusa (3 posts), github.com/beremaran/godot-agent-loop, github.com/htdt/godogen, github.com/ai-boost/awesome-harness-engineering, chierhu.medium.com survey, digitalthoughtdisruption.com (2026-07-31), summerengine.com, sorceress.games.
**Secondary sources:** decrypt.co, biggo.com/thepromptindex.com/somethingbig.ai on Matt Shumer's "Gauntlet Loop"; gummysearch.com's third-party topic summary of r/aigamedev.

---

# AI Game-Dev Context Engineering: Golden Rules

**Access note:** Direct fetches of `reddit.com` and `x.com` returned a hard block / HTTP 402 in this environment — confirmed on multiple attempts. Draws on Hacker News threads (real point/comment counts), indie-dev blogs and itch.io devlogs from people who shipped a prototype/demo, GitHub repos of AI-game-dev tooling, and press/Medium coverage.

## Scope Control

**1. Lock 2-4 hard constraints before you write a single prompt, then never relitigate them.**
The dev of *Void Balls* (shipped v0.1.0 in 2 weeks with Claude Code + Unity MCP) fixed a two-button control scheme, a locked 7-color palette, and a 10-minute run length up front — "I describe what the game should feel like... Claude handles the implementation details." *(secondary: loopedin.games)*

**2. Be willing to cut the ambitious version rather than push an AI agent through it.**
*Azure Flame Dungeon*'s dev started a 15,000-line terminal roguelike, hit a wall, and pivoted to a smaller visual scope instead of spec-patching the bigger design — framed as a deliberate scope decision, not a failure. *(secondary: itch.io devlog)*

**3. Force scope decisions into an explicit written vocabulary instead of vibes.**
Two independent open-source "AI game studio" frameworks (`gstack-game`, `Claude-Code-Game-Studios`) converged, without apparent coordination, on the same pattern: a `/prototype-slice-plan` or `/scope-check` step sorting every feature into **ADD / KEEP / DEFER / CUT** and **MUST / SHOULD / COULD** before any code is written — two independent teams landing on the same taxonomy is the strongest convergence signal in this research. *(secondary: GitHub READMEs)*

## GDD Structure

**4. Converge on a fixed-section GDD (~8 sections) rather than free-form prose.**
Both frameworks above independently standardize a GDD around core loop, systems, progression, economy, player motivation (plus 3 more, framework-specific). One (`gstack-game`) builds a `/game-import` step to coerce messy PDFs into this shape, and a `/game-review` audit that blocks progress until gaps are filled. *(secondary: GitHub)*

**5. Split the GDD (human vision) from the agent context file (build rules) — don't merge them.**
Put the *full* design document in `docs/gdd.md`, but keep `CLAUDE.md`/`AGENTS.md` limited to "game design context that **affects architecture decisions**" — state machines, save-data shape, resource formulas — excluding cosmetic/narrative material the AI can't act on anyway. *(secondary: mrphilgames.com)*

## Markdown-as-Source-of-Truth

**6. Treat the spec as the thing you edit, not the code.**
GitHub's engineering blog describes coding "entirely in Markdown," recompiling the implementation from it each time the spec changes — the same three-file discipline (`requirements.md` → `design.md` → `tasks.md`) Amazon's Kiro documents independently, used to build a real, ~95%-AI-coded infinite-crafting game (`spirit-of-kiro`). Two vendors independently shipping the same discipline is the evidence, not one blog's opinion. *(secondary: github.blog, kiro.dev, GitHub repo)*

**7. One markdown file per agent/feature, not one giant shared doc.**
On an HN thread about parallel Claude agents via tmux (**189 points, 131 comments**, directly fetched), consensus was one spec file per agent/worktree specifically to avoid merge conflicts and context bleed — 2-3 focused agents outperformed 6-8 competing ones for correctness-sensitive work.

## Context Engineering

**8. Budget the context window explicitly — split the "masterprompt" before the model forces you to.**
*Godot AI Suite*'s version history added "customization for what gets included" in its exported `Masterprompt.txt` specifically because the file "was getting too big for some LLMs[' ] context windows" on larger projects — a real dev hitting context rot and fixing it. *(secondary: itch.io devlog)*

**9. Fork/isolate context per subtask so errors don't compound across a whole build.**
*Godogen* (Show HN: **337 points, 205 comments**, directly fetched) uses an orchestrator skill plus a task-executor skill in a **forked context per task**, and a separate visual-QA agent that only ever sees screenshots (never code) to catch spatial bugs like z-fighting.

**10. Give the agent a compact, structured view of state — don't make it infer state from prose or pixels.**
A dev built a text-only renderer separate from game logic so an AI playtester agent got a dense, unambiguous game-state string (HN, **136 points, 36 comments**, directly fetched). Validating a *known* feature/bug this way was "fairly successful"; open-ended bug hunting was not — the agent didn't spontaneously try to break locked doors. Screenshot-based approaches hit a matching failure mode for real-time/physics games.

## What to Include vs. Leave Out

**11. Six things belong in almost every effective agent spec; everything else is noise.**
Citing GitHub's own analysis of 2,500+ real agent-config files (via Addy Osmani's spec-writing guide): exact runnable commands, test framework + locations, explicit directory structure, **one real code sample** (outperforms paragraphs of style description), git/commit conventions, and a tiered boundary list (✅ always / ⚠️ ask first / 🚫 never — "never commit secrets" was the single most common effective boundary). *(secondary: addyosmani.com)*

**12. Leave out vague vision language and unsummarized doc-dumps — both fail the same way.**
"Build me something cool" and a 50-page unsummarized wiki dump are two ends of one failure mode: the agent has no way to know which sentence is a constraint and which is color commentary. "Minimal doesn't mean short." *(secondary: addyosmani.com)*

## Iterative Refinement of Specs

**13. Grow the context file only when the agent actually makes a mistake — don't pre-write the perfect doc.**
"Start small, grow it... every minute spent on CLAUDE.md saves ten minutes correcting AI-generated code." *(secondary: mrphilgames.com)*

**14. Close the loop with direct observation, not description, whenever the tooling allows it.**
Three independent sources land on the same fix: Unity MCP letting Claude inspect live game state (*Void Balls*); a visual-QA sub-agent judging only screenshots (*Godogen*); a text-state playtesting harness (HN thread). Describing results to the agent is weaker than letting it *observe* results.

**15. Replace "the AI will get it right" with automated conformance/quality gates.**
*Azure Flame Dungeon*'s dev: **"Quality requires systems, not hope. 'The AI will get it right' is not a quality strategy. Automated testing is."** Paired with continuous human playtesting for feel/pacing.

**16. The bottleneck is vision, not code generation — spend iteration budget clarifying intent, not re-prompting for syntax.**
Same dev: "The hardest part isn't code — it's vision. Knowing what you want is harder than making it happen."

**Cross-source patterns worth trusting more than any single post:** the 8-section GDD structure and ADD/KEEP/DEFER/CUT scope taxonomy appeared independently in two unrelated open-source frameworks; splitting human-facing GDD from agent-facing context file, and giving the agent a way to observe real state rather than read about it, each showed up across 3+ independent sources. Every source reporting a *finished*, playable outcome mentioned deliberate scope-narrowing in week one, and none described writing a comprehensive GDD before starting — the doc grew alongside the build.

**Sources:** [mrphilgames.com](https://www.mrphilgames.com/blog/claude-md-for-game-devs) · [HN 189pt/131c](https://news.ycombinator.com/item?id=47218318) · [HN Godogen 337pt/205c](https://news.ycombinator.com/item?id=47400868) · [HN 136pt/36c](https://news.ycombinator.com/item?id=47947525) · [loopedin.games — Void Balls](https://www.loopedin.games/blog/how-i-built-void-balls-using-ai/) · [Azure Flame Dungeon devlog](https://yurukusa.itch.io/azure-flame-dungeon/devlog/1385242/how-this-game-was-built-entirely-by-ai-and-what-that-actually-means) · [creatoreconomy.so](https://creatoreconomy.so/p/build-a-retro-game-with-claude-code-in-20-min) · [gstack-game](https://github.com/fagemx/gstack-game) · [Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) · [Godot AI Suite devlog](https://marcengelgamedevelopment.itch.io/godot-ai-suite/devlog) · [Addy Osmani — good spec](https://addyosmani.com/blog/good-spec/) · [GitHub Blog — Markdown as a programming language](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/) · [Kiro specs best practices](https://kiro.dev/docs/specs/best-practices/) · [spirit-of-kiro](https://github.com/kirodotdev/spirit-of-kiro) · [Cobus Greyling — AGENTS.md](https://cobusgreyling.substack.com/p/what-is-agentsmd)

---

# Visual (and Audio) Assets: Golden Rules

**Access note:** reddit.com (www and old.reddit.com) returned "unable to fetch" and x.com/twitter.com returned HTTP 402 Payment Required — confirmed blocked. Secondary-sourced: press quoting specific Reddit/X posts verbatim (Kotaku, PCGamesN, Notebookcheck, indiecator.org), an HN Show HN thread verified directly via the HN Algolia API (real points/comments), an itch.io community forum thread fetched directly, independent dev blogs, and GitHub tool/skill repos. Generic "best AI tools 2026" listicles with no first-person account and no cross-source corroboration were dropped or flagged as thin.

**Cross-check:** this user's own FI-GRAPHICS doctrine (code-drawn for systematic art, image-gen only for singular art; approve only full in-game screenshots; chroma-key/quantize pipeline), already logged in Gestalt, is independently confirmed rather than contradicted by this research — noted inline.

## Golden Rules

**1. Route by runtime job, not by model hype.**
Decide what the asset *does* in the game (static prop vs. animated combat sprite vs. skybox) before picking a tool — each has a different reliable pipeline. Treating every asset as "just prompt it" is the single biggest cause of wasted regenerations.
*Tools: `ai-game-art-pipeline-skill` (GitHub: ybuild-ai) codifies this as its core rule. StraySpark's 2026 ComfyUI playbook independently arrives at the same five-workflow split (concept sheets, tileable PBR, sprite/animation frames, skybox HDRs, upscaling).*

**2. Reuse the canonical asset before you regenerate it.**
Once a character/prop has an approved sprite sheet, edit or reuse it — don't re-roll from scratch for every new pose. Diffusion "reroll" pipelines drift in face/proportions across separate calls; edit-based models don't.
*Gemini 2.5/3.1 Flash Image ("Nano Banana"/"Nano Banana Pro") is repeatedly cited as the 2026 breakout tool specifically because it edits a held character across scenes instead of re-generating it.*

**3. Build a reference/turnaround sheet first; generate individual frames against it, don't ask for one big grid.**
Split content across multiple sheets (3–6 items/row, 2–3 rows max) with hyper-specific per-cell descriptions rather than one vague prompt.
*Source: roboticape.com devlog — 48 clothing sprites + 26 character sprites, half a day total including manual QA, a genuinely shipped-pipeline account. StraySpark's concept-sheet workflow (FLUX → IP-Adapter style anchor → ControlNet OpenPose four-view rig) mirrors this at the 3D/turnaround level.*

**4. Lock style with a trained LoRA or a style-bible prompt suffix — never re-describe style per prompt.**
Train a small LoRA (10–30 reference images, ~15–30 min on consumer hardware via AI Toolkit/Kohya, the "Ostris method") rather than retyping style keywords every time.
*LoRA + ControlNet OpenPose (pose) + IP-Adapter (style anchor) — StraySpark's ComfyUI pipeline; version-controlled workflow JSONs + fixed seeds for reproducible multi-person results.*

**5. Transparency/alpha is a solved VFX problem — don't ask the model for it, chroma-key it yourself.**
Image models cannot output true alpha channels; naive dark/magenta-background threshold removal fails (anti-aliasing bakes background color into edges). Working recipe: pure `#00FF00` background + explicit 2–3px white outline in the prompt, ban gradients/noise/shadows, then **HSV-based** chroma-key removal (hue ±22° of 120°, sat/value thresholds).
*Source: roboticape.com, first-hand. Independently matches this user's existing FI pipeline (chroma-key, border-connected, hue-based, brightness-independent → crop → quantize) — strong cross-validation.*

**6. Use a vision-model screenshot loop for technical QA, but don't expect it to catch aesthetics.**
Live in-engine screenshots fed to a vision model (Gemini Flash) reliably catch technical breakage — z-fighting, missing textures, broken physics — in an autonomous fix-iterate loop, but do **not** catch "this looks lifeless" / game-feel problems.
*Source: Godogen — its own retrospective noted the visual QA loop caught technical defects but demo output still read as "lifeless" to observers. Treat AI critique loops as a defect filter, not an art director.*

**7. Never let raw generator output land directly in the engine repo — stage it, then review full-frame, then import.**
Route generated files to a staging folder, require a human review step, and approve only from full-size in-game/composite screenshots, never thumbnails or crops.
*Sources: StraySpark's production pipeline description; `sheet_contact.py`-style contact sheets in `ai-game-art-pipeline-skill`. Matches this user's own "approval only on full-size in-game screenshots" rule already in Gestalt.*

**8. The real bottleneck isn't generation, it's the import-and-setup gap — close it with in-editor tooling.**
Generation itself is fast and often good; the expensive part is getting the file to land correctly in the engine and survive the renderer. This is why 2026 tooling investment went into in-editor/agent-driven integration (Unity Muse Chat/Animate/Texture; Godot's text-first scene/script format; MCP-based engine tools) more than generation quality itself.

**9. Know the specific "reads as AI" tells and screen for them before anyone else does.**
Real community feedback (itch.io "Let's Talk About AI in Game Dev" thread, fetched directly) names concrete tells: missing/extra fingers, floating torsos/limbs, and — for pixel art — an "uncanny tried-to-be-pixel-art-but-doesn't-know-what-pixel-art-is look." Commenters recommended using AI output only as a compositional reference plus manual touch-up, or building covers from actual in-game screenshots instead.
*Matches this user's own already-logged quality-trap list (tiling artifacts, perspective mismatch, slicing into wrong-size slots, filename/label scramble) — independent confirmation.*

**10. Disclose AI use deliberately, and separate "AI-assisted" from "asset-flipped" in your own head.**
Steam Next Fest Feb 2026 (3,455+ demos, ~1,694 disclosing AI content) triggered visible backlash: Palworld dev John "Bucky" Buckley posted on X that AI capsule art was "an immediate turn off" (quoted by PCGamesN); a Reddit user (u/SomeRandomArtist31, r/Steam, via PCGamesN) wrote "I wish there was a way to filter out games that use generative AI... I don't want to play them." Same coverage documents a real false-positive problem: legitimate solo devs report being wrongly accused of AI use because hand-made art coincidentally resembles the AI "look" — reinforcing rule 9 and making disclosure hygiene a real shipping-day task.
*Sources: PCGamesN, Kotaku, Notebookcheck, indiecator.org (June 16 2026) — same event from different angles, a repeated pattern, not a one-off claim.*

**What didn't hold up:** several SEO/content-mill sites (thinkpeak.ai, apatero.com, techsy.io, nastyrodent.com) repeated generic claims ("Leonardo.Ai is best for X," "50% of studios use AI") with no first-person account and no cross-source corroboration — excluded except where independently confirmed. One specific claim (an "indie dev @PixelForge on X" 16-pose orc-sheet anecdote) appeared in exactly one low-quality source with no corroboration anywhere — flagged as likely fabricated, do not treat as real.

**Sources:** [roboticape.com — Nano Banana sprites](https://roboticape.com/2026/03/07/generating-game-sprites-with-gemini-image-generation-nano-banana-pro-lessons-learned/) · [StraySpark — ComfyUI 2026 playbook](https://www.strayspark.studio/blog/comfyui-game-asset-pipeline-indie-2026) · [ai-game-art-pipeline-skill](https://github.com/ybuild-ai/ai-game-art-pipeline-skill) · [Godogen coverage](https://agent-wars.com/news/2026-03-16-godogen-godot-4-game-generator-claude-code) / [repo](https://github.com/htdt/godogen) · [itch.io forum thread](https://itch.io/t/4975370/lets-talk-about-ai-in-game-dev) · [PCGamesN](https://www.pcgamesn.com/steam/next-fest-2026-generative-ai) · [Kotaku](https://kotaku.com/steam-next-fest-feb-2026-gen-ai-art-demos-2000673280) · [Notebookcheck](https://www.notebookcheck.net/Steam-Next-Fest-flooded-with-AI-slop-and-low-effort-demos-users-tell-Valve.1233436.0.html) · [indiecator.org](https://indiecator.org/2026/06/16/steam-next-fest-has-an-ai-problem-and-players-cant-filter-it-out/) · [HN Show HN 319pt/152c](https://news.ycombinator.com/item?id=44463967)

---

# Hosting, Publishing & Marketing: Golden Rules

**Access note:** reddit.com (including old.reddit.com) and x.com/twitter.com could not be fetched directly — reddit returns a hard block, x.com returns HTTP 402. WebSearch also failed to surface indexed reddit thread text with depth. Secondary-sourced: dev blogs/devlogs, itch.io devlog posts, press (PC Gamer, GamesRadar, Kotaku, TechSpot, PCGamesN, The Gamer), Steam store pages, and one primary blog post by Pieter Levels (levels.io, directly fetched). Single-anecdote claims are flagged as weaker than patterns repeated across 3+ independent sources.

## The Golden Rules

**1. Disclose AI use deliberately and specifically — never minimally.**
Steam's Jan 2026 rule change replaced the binary AI checkbox with a two-tier system: Tier 1 (pre-generated static assets) needs only a checkbox; Tier 2 (AI generating content live at runtime) triggers deep review. **"Developer efficiency tools" are exempt** — coding assistants, MCP/editor automation, non-AI procedural generation don't require disclosure if the player never directly experiences the raw AI output. Among games that *do* disclose, flopped AI-tagged titles (~50 reviews) had median disclosure text of **13 words**; successful ones (~1,000+ reviews) wrote detailed, reassuring disclosure explaining *how* AI fit into a human-led process. **Rule: if you disclose, over-explain, don't under-explain.**
*(fragwyz.substack.com data, corroborated by soonlab.ai and StraySpark)*

**2. Never lead marketing with AI-generated concept art or a non-playable trailer.**
The most repeated failure pattern across press coverage: an AI-generated concept clip goes viral (one hit 25M views in a day), backlash follows within hours once people realize there's no playable game, and AI-assisted knockoffs ship before the original creator does (happened to a real Freya Holmér prototype). **Rule: only market what's actually playable** — open trailers with 30-45 seconds of real, in-engine gameplay in the first 2-5 seconds.

**3. Treat AI as infrastructure in the pitch, never as the headline feature.**
Across every success/failure comparison found, games using AI for voice synthesis, localization, code scaffolding, and asset iteration — while keeping human-authored creative vision front and center — outperformed games leaning on AI-generated visuals as the selling point (72% of flops were art-gen-only). "AI is the sophisticated brush, not the painter."

**4. Expect an organized anti-AI-slop audience, and build store-page trust signals against it.**
Steam Next Fest 2026 had ~8,600+ demos, ~1,700 (roughly 1 in 5) with AI disclosures — this triggered real tooling: browser extensions that flag/filter AI-disclosed games in discovery, plus vocal sentiment that some players auto-blacklist any AI-tagged game (via PCGamesN, Kotaku, The Gamer, ResetEra). Counter with consistent art style, devlogs showing sketches/iteration, and specific/opinionated store copy (vague copy itself reads as AI-generated).

**5. Store page optimization has a proven, numbers-backed playbook — run it.**
One documented 90-day case study (gamineai.com): capsule art redesigned for legibility at 184px took conversion 1.6%→3.2%; tag audit (9 tags/3 accurate → 15 tags/11 accurate) took weekly impressions 3,200→5,800; trailer re-cut (strongest frame at 0:00-0:02, logo card moved to end, 110s→60s) took conversion 3.8%→4.6%; a 25-minute vertical-slice demo + 4-touch announcement cadence got 481 downloads at 64% wishlist conversion. Net: 284→1,047 wishlists over 90 days, zero ad spend. Calibration context: wishlist-to-sale conversion has fallen from ~20% (2018) to **5-10%** (2026); ~30,000 wishlists at launch is the realistic floor for discovery-algorithm traction; only ~6% of new games hit 100,000 pre-launch wishlists.

**6. Pick your platform by what it's actually for, not by default.**
Steam = the money platform, but pay an "AI tax" in review scrutiny; best for long-term-supported games. itch.io = community discovery/iteration, devlog culture rewards consistency over budget. Web portals (CrazyGames ≤50MB first download/≤20MB mobile-homepage; Poki ≤8MB) use a basic-launch→full-launch (revenue-sharing) gate based on player response. Self-hosted web (own domain) gives press-kit-ready control but you own CORS/same-origin headaches — only pays off once you already have traffic.

**7. Start community-building 12-18 months before launch, not at launch.**
The single most common mistake: treating community-building as a launch-week activity. Anchor on Discord, give early members a "Founder" role, set up the Steam page early purely to collect wishlists, and critically — do not go quiet in the final months before launch, exactly when word-of-mouth momentum dies.

**8. On Reddit specifically, follow the unwritten 90/10 rule and weekly cadence.**
r/aigamedev, r/IndieDev, r/itchio norms: roughly one self-promo post per week, short and specific (title + 2-3 screenshots or a 30-60s clip + link), flair used correctly, genuine discussion value added. Broader norm: ~90% genuine participation, ≤10% self-promotion. Gaming this (sockpuppets, identical cross-posting) burns the account/community relationship.

**9. On X specifically, #screenshotsaturday is still the reliable recurring hook.**
Weekly #screenshotsaturday posts (real progress screenshot/GIF) plus broader tags (#indiegame, #gamedev) remain the standard cadence tactic; engagement compounds from replying/engaging with other devs, not broadcast volume.

**10. The realistic proof points, not the hype.**
*Fire Field*: solo dev (Naoki Fujinaga, no prior gamedev experience) shipped a commercial Diablo-like ARPG in ~3 months using Claude Code as primary coding partner, ~120,000 lines generated, custom engine from scratch — press coverage centered the *process*, not the tooling. *AI2U: With You 'Til The End*: AI-generated dialogue/behavior as a core, disclosed mechanic, Early Access since Jan 2025, sustained 89% positive across 1,658 reviews by 1.0 — proof AI-as-core-mechanic can work, but only with sustained live-service iteration. Vibe Jam 2026 (Pieter Levels, judged by Tim Soret): winners were simple, well-executed concepts (a capybara food-delivery game took 1st), not technically flashy AI showcases. Broader market data: ~1 in 3 new Steam releases carries an AI disclosure but that third earns only ~10% of category revenue — AI-disclosed games trail non-AI games by ~45% in per-game success rate. Shipping with AI does not, by itself, improve your odds.

## Condensed operating checklist
1. Disclose AI use in detail if you disclose at all — never a throwaway line.
2. Never market with AI concept art/trailers that outpace what's actually playable.
3. Position AI as infrastructure (voice, localization, code, iteration speed), not the pitch.
4. Keep visual/art style consistent; write specific, opinionated store copy.
5. Run the Steam page playbook: legible capsule at 184px → genre-accurate tags → gameplay-first trailer → vertical-slice demo → staged announcement cadence.
6. Set wishlist goals against 2026 reality (5-10% conversion, ~30k at launch = floor).
7. Choose platform by purpose: Steam (durable monetization + scrutiny), itch (community/iteration), CrazyGames/Poki (lean instant-play, respect size caps), own domain (once you have traffic).
8. Start Discord/community 12-18 months out; never go dark pre-launch.
9. Reddit: ~weekly self-promo, 90/10 rule. X: weekly #screenshotsaturday, engage don't just broadcast.
10. Study Fire Field (process story) and AI2U (sustained live-service disclosure) over one-off viral AI demo reels.

Logged to Gestalt as `ai-game-shipping-golden-rules` for reuse across FI Arcade titles (Crookémon, 2342, AI Prompt Simulator, Confluence); cross-referenced against `fi-brand-message` (anti-extraction/anti-corporate, mission-forward voice pairs with rules #3/#4 above).
