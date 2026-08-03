# Golden Rules for specs that AI agents can actually build from

- **Window:** June 7 – August 3, 2026  
- **Focus:** Spec structure, GDD vs implementation docs, precision, non-goals, task context packets  
- **Related in this repo:**  
  - [Agent workflows: emerging consensus](2026-08-agent-workflows-emerging-consensus.md)  
  - [Agent harness golden rules](2026-08-agent-harness-golden-rules.md)  
  - [docs/golden-rules-ai-gamedev.md](../golden-rules-ai-gamedev.md)  
- **Sources:** Citations at end of this file  

I prioritized discussions from **June 7–August 3, 2026**, especially first-hand reports tied to playable prototypes, closed playtests, or detailed failure postmortems. The clearest pattern is that successful developers provide **less total vision but more operational precision**.

---

## 1. Specify one playable slice, not the whole dream

**Rule:** The active specification should describe the smallest end-to-end version that proves the game’s core experience.

**Why it works:** Agents treat documented future ideas as potential current work. A recent Godot builder put the MVP beside 27 future feature sections, then repeatedly said “continue.” Within hours the agent implemented most of the roadmap, producing 42 scripts, hundreds of assets, a 1,359-line world script, and a build running around 3 FPS. ([Reddit][1])

**Evidence from prototypes:** The developer of *Day 31*, whose first FPS level reached closed playtest, deliberately limited the build to combat feel, an infection timer, and one pursuing enemy. The other designed floors remained outside the current implementation target. A separate one-hour incremental prototype succeeded because the creator supplied one explicit loop—damage, earn, upgrade, automate, prestige—and a fixed time and scope budget. ([Reddit][2])

**Practical pattern:**

```markdown
## Current vertical slice
- One level
- One complete player loop
- One enemy family
- One progression decision
- One win condition
- One lose condition

## Explicitly excluded
- Meta progression
- Additional biomes
- Multiplayer
- Cosmetic customization
- Procedural campaign
```

Future features belong in a backlog that the implementation agent is not instructed to read.

---

## 2. Use a stable GDD plus small implementation specs

**Rule:** Separate the enduring game definition from the document that tells an agent what to build today.

**Why it works:** A single giant GDD mixes vision, possibilities, architecture, tasks, lore, and current requirements. Agents struggle to distinguish mandatory instructions from inspirational material.

**Evidence from prototypes:** The *Day 31* developer used Claude as a “design brain” that wrote tight Markdown specifications for each system, while a separate Unity-connected implementation agent executed them. The builder reported that quality fell quickly whenever the implementation agent was allowed to make design decisions; execution improved when each spec referenced actual component names and APIs. ([Reddit][2])

A useful hierarchy is:

```text
GAME.md
    Stable player fantasy, core loop, pillars and non-goals

SYSTEMS/
    combat.md
    enemies.md
    progression.md
    save-system.md

MILESTONE.md
    Only the current vertical slice

TASKS.md
    Small dependency-ordered work items

ACCEPTANCE.md
    Observable completion conditions

DECISIONS.md
    Locked choices and their rationale

BACKLOG.md
    Ideas not authorized for implementation
```

`GAME.md` changes rarely. System documents change when design changes. `MILESTONE.md` and `TASKS.md` change continuously.

---

## 3. Make Markdown authoritative—but do not make every file authoritative

**Rule:** Give every document one clearly defined responsibility and state which file wins when documents conflict.

**Why it works:** Markdown is readable by humans, agents, version control, scripts, and multiple coding assistants. But five overlapping sources of truth are worse than no source of truth.

**Evidence from recent practice:** One developer described maintaining two editable Markdown files: a higher-level history and deadline document, plus a detailed task document containing small daily work items. They favored ordinary human-readable Markdown because humans and multiple agents could update it without format friction, and considered regularly reviewed project documents more important than continuously expanding `CLAUDE.md` or `AGENTS.md`. ([Reddit][3])

Add precedence explicitly:

```markdown
## Document precedence

1. ACCEPTANCE.md defines whether work is complete.
2. MILESTONE.md defines current scope.
3. SYSTEMS/*.md defines system behavior.
4. GAME.md defines product intent.
5. BACKLOG.md is informational and must not be implemented.
```

Use `AGENTS.md` or `CLAUDE.md` for repository-wide operating rules, such as build commands, forbidden directories and coding conventions—not for the entire game design.

---

## 4. Be precise where invention affects gameplay

**Rule:** Provide concrete values whenever the agent’s guess would materially change balance, progression, logic, scope, or success.

**Why it works:** Words such as “challenging,” “fast,” “balanced,” and “occasionally” do not produce consistent implementations. They force the model to become the game designer.

A recent r/aigamedev workflow for generating playable prototypes recommended concrete values for enemy stats, wave composition, upgrade costs, resource rates, starting values, and win/lose conditions. It recommended only directional guidance for areas where reasonable defaults are acceptable, such as broad visual style, camera behavior, interaction model, and audio approach. ([Reddit][4])

**Specify exactly:**

- Health, damage, speed and cooldowns.
- Economy inputs and outputs.
- Spawn counts and ordering.
- State transitions and triggers.
- Win, loss and restart conditions.
- What persists after death or saving.
- Performance or size budgets.
- Required UI information.

**Leave directional:**

- Exact decorative colors.
- Minor animation timings.
- Incidental particles.
- Individual key bindings when rebinding is allowed.
- Decorative environment placement.
- Flavor text that does not affect logic.

A strong instruction is:

> Be precise wherever the implementer would otherwise invent a game rule; be directional wherever a conventional default is safe.

---

## 5. Put non-goals and stop conditions beside the goals

**Rule:** Every milestone and prompt should say what the agent must not do and when it must stop.

**Why it works:** Agents naturally interpret ambiguity as permission to continue being helpful. “Continue” is therefore a scope-expansion command.

The 27-system postmortem explicitly traced its scope explosion to a design document containing the long-term architecture and feature list, combined with instructions that never required the agent to stop after the MVP or restrict which files it could modify. ([Reddit][1])

A June X experiment demonstrated the same problem at larger scale: an open-ended GTA-style loop scaffolded features extremely quickly but created the wrong city by its second day and later migrated engines. That is impressive generation velocity, but also clear goal and technology drift. ([GTA BOOM][5])

Every work order should contain:

```markdown
## Stop when
- The listed acceptance checks pass.
- No unlisted feature is required.
- Do not begin the next backlog item.
- Report unresolved questions instead of inventing scope.

## Do not change
- Engine or rendering pipeline
- Save-data schema
- Input architecture
- Files outside the listed paths
```

---

## 6. Give the agent a task-sized context packet

**Rule:** Supply the smallest set of current facts needed to execute the task correctly.

**Why it works:** More context is not automatically better context. Old plans, abandoned alternatives, entire chat histories, speculative lore, and unrelated source files create conflicting cues.

For each task, provide:

1. The current objective.
2. The relevant system specification.
3. Existing component, scene and API names.
4. Files it may inspect or modify.
5. Constraints and non-goals.
6. Acceptance criteria.
7. Known failures or prior attempts.

The *Day 31* developer found that Markdown specs worked best when grounded in the real codebase rather than abstract descriptions. For example, the document named existing components and APIs the Unity implementation agent should use. ([Reddit][2])

A good context header looks like:

```markdown
## Existing implementation
- PlayerCombat.cs owns attack initiation.
- HitResolver.cs applies damage.
- EnemyReactionController.cs owns reactions.
- Do not add a second damage pipeline.

## Files in scope
- PlayerCombat.cs
- HitResolver.cs
- EnemyReactionController.cs
- CombatAcceptanceTests.cs
```

This prevents parallel systems and invented architecture.

---

## 7. Describe observable behavior, not emotional dissatisfaction

**Rule:** Convert taste judgments into symptoms, causes, and desired observable changes.

**Why it works:** “Make combat punchier” gives the agent no reliable diagnosis. It may add camera shake, particles, larger numbers, sound, animation changes, or all of them.

The *Day 31* builder reported that saying melee “feels dead” produced little value. Describing the actual defects—no visible enemy reaction and hit-stop triggering only on kills—produced a one-pass fix. ([Reddit][2])

Use this structure:

```markdown
Current observation:
Normal hits produce no visible enemy response.

Expected observation:
Every confirmed melee hit interrupts locomotion for 120 ms and plays
one directional reaction before locomotion resumes.

Not requested:
Do not change damage, attack speed, camera shake or audio.
```

This is particularly important for:

- Combat feel.
- Movement.
- Camera behavior.
- UI clarity.
- Tutorial friction.
- Enemy readability.
- Pacing and progression.

---

## 8. Let the GDD record decisions, not generate new ones

**Rule:** When converting brainstorming into a specification, instruct the model to act as a faithful editor rather than an additional designer.

**Why it works:** Models routinely fill perceived gaps with plausible systems. Those additions can silently become requirements when the document is later handed to an implementation agent.

A recent prototype workflow first discussed mechanics conversationally, then asked a separate step to produce the GDD while explicitly forbidding editorializing or adding systems that had not been discussed. Missing decisions were collected at the end rather than silently invented. ([Reddit][4])

A useful GDD-generation instruction is:

```text
Convert the approved discussion into a specification.

Do not add mechanics, content, progression systems or technical
requirements that were not explicitly approved.

When information is missing:
- mark it as an open decision;
- explain why implementation depends on it;
- do not choose an answer yourself.
```

This preserves human authorship and prevents “helpful” scope growth.

---

## 9. Use a fixed skeleton with a game-specific middle

**Rule:** Standardize the universal parts of the GDD, but create system sections only for mechanics the game actually contains.

**Why it works:** A completely freeform document omits basic implementation information. A massive universal template encourages unnecessary systems simply because a heading exists for them.

A recent r/aigamedev prototype workflow recommended a fixed outer structure—technology, concept, visual direction, camera, controls, core loop, UI, audio and first vertical slice—with a variable middle containing only the game’s actual major systems. ([Reddit][4])

A compact agent-ready GDD structure:

```markdown
# Game concept
One paragraph: genre, player fantasy and tone.

# Design pillars
Three to five rules used to reject features.

# Core gameplay loop
Numbered sequence from entry through win/loss/restart.

# Platform and technical constraints
Engine, target devices, input and performance budget.

# Camera and controls
Interaction model and required capabilities.

# Major systems
Only systems present in the current design.

# UI information model
What the player must know and when.

# Content boundaries
Required content counts for the current slice.

# Vertical slice
Concrete first implementation target.

# Non-goals
Features explicitly deferred or rejected.

# Acceptance criteria
Observable behaviors proving completion.

# Open decisions
Questions requiring human judgment.
```

---

## 10. Prototype mechanics with placeholders; polish only what the hypothesis requires

**Rule:** Match specification detail to the question the prototype is meant to answer.

**Why it works:** A prototype testing movement does not need final lore, dozens of enemies, polished menus, or a finished asset pipeline. Conversely, a game whose appeal is fashion, atmosphere, animation, or visual transformation may need representative art early.

Recent developers recommended proving the core loop with basic shapes when mechanics are the value proposition, then replacing them later. They recommended near-representative visuals earlier when visual appeal itself is the product hypothesis. ([Reddit][3])

Ask one question per prototype:

- Is movement satisfying?
- Is the deduction solvable and fair?
- Is the progression interesting for ten minutes?
- Is the enemy readable?
- Can players understand the loop without explanation?
- Does the intended art direction support the fantasy?

Anything that does not help answer that question is probably unnecessary detail.

---

## 11. Refine the specification from play evidence, not agent enthusiasm

**Rule:** Update documents only after builds, tests, play sessions, or human decisions reveal new facts.

**Why it works:** Agents can confidently produce a detailed plan for a game that is not fun. The specification should evolve from observed behavior, not merely from increasingly elaborate conversations.

The one-hour incremental prototype demonstrated the division clearly: AI rapidly scaffolded UI and repetitive code, but balance, pacing and progression feel still required human judgment and player feedback. ([Reddit][6])

The playable *Franky Trends* prototype used specialized writer, verifier and automated playtester agents in a repeating generate → critique → fix → playtest loop. Verifiers checked clue consistency and solvability; the playtester interacted with the actual game and reported confusion, friction and visual discrepancies. ([Reddit][7])

A reliable refinement cycle is:

```text
Brainstorm
→ human approves decisions
→ write or update specification
→ implement one slice
→ build and play
→ record observations
→ change the specification
→ implement the next revision
```

Do not rewrite the whole GDD after every new idea. Record the idea in `BACKLOG.md`, then promote it only after an explicit design decision.

---

# The optimal amount of detail

The current evidence suggests a useful boundary:

> **Enough detail that the agent does not need to invent rules; little enough detail that only the current hypothesis becomes work.**

### Include

- One-sentence concept.
- Player fantasy.
- Three to five design pillars.
- Core loop.
- Current vertical slice.
- Concrete mechanics and numbers.
- State transitions.
- Win and loss conditions.
- Platform and performance constraints.
- Existing architecture and API names.
- Required UI information.
- Acceptance criteria.
- Non-goals and protected files.
- Explicit open questions.

### Leave out of the active context

- Exhaustive lore not used by the current slice.
- Every possible future mechanic.
- Multiple unresolved design alternatives.
- Long chat transcripts.
- Abandoned architecture.
- Generic engine documentation the agent can retrieve.
- Decorative content lists.
- Vague goals such as “make it polished.”
- Comparisons like “Zelda meets Diablo” without translating them into mechanics.
- `TBD`, “roughly,” or “scale appropriately” where gameplay depends on the answer.
- Backlog items that the agent could mistake for authorization.

---

# Recommended prompt format

```markdown
# Objective
Implement enemy hit reactions for the current combat slice.

# Product intent
Hits must be immediately readable without making combat slower.

# Existing implementation
- Damage is applied by HitResolver.
- Enemy locomotion is owned by EnemyMotor.
- Reactions are owned by EnemyReactionController.

# Required behavior
- Every confirmed normal hit pauses enemy locomotion for 120 ms.
- Play the reaction matching the incoming hit direction.
- Bosses use the same reaction but are not interrupted.
- Death behavior remains unchanged.

# Files allowed
- EnemyReactionController.cs
- EnemyMotor.cs
- CombatAcceptanceTests.cs

# Non-goals
- Do not change damage values.
- Do not add camera shake.
- Do not modify player attacks.
- Do not create a new combat manager.

# Acceptance criteria
- Normal enemies visibly react to ten consecutive valid hits.
- Boss movement is never interrupted.
- Misses trigger no reaction.
- Existing combat tests pass.

# Stop condition
Stop after the criteria pass. Report any remaining failure.
Do not begin another combat feature.
```

The recurring lesson from finished prototypes is that **the best prompt is rarely a larger creative brief**. It is a small, current, code-aware contract nested inside a stable human-owned design system.

---

## Citations

[1]: https://www.reddit.com/r/aigamedev/comments/1va3loa/how_one_mvp_turned_into_27_systems/ "How One MVP Turned Into 27 Systems : r/aigamedev"

[2]: https://www.reddit.com/r/aigamedev/comments/1uo7fpz/i_built_a_horror_fps_using_claude_as_my_design/ "I built a horror FPS using Claude as my design brain + an MCP server implementing in Unity. The first level just hit closed playtest : r/aigamedev"

[3]: https://www.reddit.com/r/aigamedev/comments/1vdjntu/how_do_you_plan_an_aiassisted_game_when_the/ "How do you plan an AI-assisted game when the mechanics and art direction keep changing? : r/aigamedev"

[4]: https://www.reddit.com/r/aigamedev/comments/1up0b50/explain_your_one_shot_prompts/ "Explain Your One Shot Prompts : r/aigamedev"

[5]: https://www.gtaboom.com/someone-is-trying-to-vibe-code-a-gta-6-clone-with-ai-agents-before-rockstar-ships-the-real-one-0838 "Someone Is Trying to Vibe Code a GTA 6 Clone With AI"

[6]: https://www.reddit.com/r/aigamedev/comments/1ualqhq/testing_out_ai_for_incremental_game_development/ "Testing out AI for Incremental Game Development in 2026 - I made Punching Bag Incremental in 1 hour : r/aigamedev"

[7]: https://www.reddit.com/r/vibecoding/comments/1tzgwj5/how_far_can_ai_agents_actually_go_in_making_a/ "How far can AI agents actually go in making a game? … prototype. : r/vibecoding"
