# AI game-dev agent workflows: the emerging consensus

- **Window:** ~June 7 – August 3, 2026  
- **Focus:** What high-signal builders actually do — demos, measurements, architecture, candid failures  
- **Related in this repo:**  
  - [Agent harness golden rules](2026-08-agent-harness-golden-rules.md) (harness/MCP/loop engineering)  
  - [docs/golden-rules-ai-gamedev.md](../golden-rules-ai-gamedev.md) (broader gamedev field notes)  
- **Sources:** Inline citations at end of this file  

I reviewed recent, high-signal discussions from roughly **June 7 through August 3, 2026**, prioritizing posts that included playable demos, concrete measurements, architecture details, or candid failure reports.

The strongest recurring conclusion is:

> **Successful builders do not maximize agent autonomy. They construct a deterministic, testable environment in which limited autonomy is useful.**

The winning architecture is a **bounded agent inside a durable harness**: persistent repo state, small tasks, executable success criteria, independent verification, and hard external limits.

---

## Golden Rule 1: Build systems, not “a game”

**The rule:** Ask the agent to build reusable game systems, data models, and interfaces—not to implement a succession of isolated features.

**Why it works:** One-shot prototypes usually optimize for visible output. They tend to produce tightly coupled scripts, duplicated logic, giant files, and architecture that becomes difficult for both humans and agents to understand. Several recent r/aigamedev discussions independently recommended defining what a weapon, event, map, notification, NPC, or inventory system is before generating individual instances of it. ([Reddit][1])

**Patterns used by builders:**

- A design document describing systems and their relationships.
- A dependency-ordered roadmap.
- Decoupled core libraries for simulation and data.
- Data models before content-heavy RPG or strategy features.
- Event managers, notification managers, editors, factories, and configurable resources instead of hard-coded instances.
- Subphases deliberately sized to fit inside one agent session.

This is the difference between “generate a weapon” and “create a weapon system whose behavior, acquisition, presentation, balance variables, and extension points are explicit.”

---

## Golden Rule 2: Treat the repository as the harness—and its durable memory

**The rule:** Important state must live in files, tests, commits, and trackers—not in the conversation history.

**Why it works:** Long-running agents lose detail through session boundaries and context compression. Recent X discussion on long-running agents repeatedly emphasized that the model forgets between runs, while the repository persists. Recent builders described using shared facts files, persistent sessions, progress documents, and roadmaps to carry state between Claude, Codex, and other workers. ([X (formerly Twitter)][2])

A practical state file should remain small:

```text
Current objective
Completed work
Current facts
Decisions and rationale
Open questions
Known failures
Files currently in scope
Next executable task
Verification still required
```

Do not turn `CLAUDE.md` or `AGENTS.md` into an encyclopedia. Recent X discussion recommends keeping always-loaded instructions focused on non-obvious operational hazards, while moving repeatable workflows into skills, hooks, scripts, or scoped subsystem instructions. ([X (formerly Twitter)][3])

---

## Golden Rule 3: Loop over bounded tasks, not elapsed time

**The rule:** The fundamental unit of autonomy should be a small, dependency-aware task followed by verification—not “run for six hours” or “keep improving the game.”

**Why it works:** Time-based autonomy encourages agents to discover or invent work, expand scope, repeatedly refactor, and compound unnoticed regressions. Builders reporting better outcomes instead break roadmaps into subphases that can be completed in one context window, then run and play the game before continuing. ([Reddit][1])

A productive loop is:

```text
Load current state
Select one ready subphase
Plan the change
Implement
Run deterministic checks
Run independent review
Playtest the affected behavior
Record evidence
Commit or roll back
Update state
Stop
```

Human checkpoints should occur at meaningful design boundaries: after a new mechanic becomes playable, before a major migration, before changing architecture, and whenever an agent’s assumptions affect game feel.

---

## Golden Rule 4: Define “done” before implementation

**The rule:** Write acceptance criteria and evaluation mechanisms before the agent writes the feature.

**Why it works:** When the same session implements and defines the audit, it can construct tests around whatever it happened to build. One recent Godot project expanded into 27 systems and marked every audit section as passed, despite later running at roughly 3 FPS. The author concluded that tests and acceptance criteria should have existed separately before implementation. ([Reddit][4])

A successful tool call is also not evidence of a successful outcome. Recent production-agent discussion distinguishes “the tool returned successfully” from “the intended result was independently verified.” ([Reddit][5])

Good game-dev criteria are executable or measurable:

- “All acceptance scenes load without errors.”
- “Ten pedestrians increase their distance from an explosion after five seconds.”
- “Saving and reloading produces an equivalent world-state hash.”
- “The player can complete the tutorial using only visible instructions.”
- “No frame exceeds the agreed simulation budget in the benchmark scene.”
- “All dialogue paths retain a logically valid solution.”
- “The screenshot differs from the reference by less than the chosen threshold.”

Recent agent-loop discussion specifically recommends goal conditions that a tool can inspect rather than criteria requiring the agent to “eyeball” whether something seems correct. ([Reddit][6])

---

## Golden Rule 5: The maker must not be the sole checker

**The rule:** Separate implementation, criticism, and playtesting into distinct roles or contexts.

**Why it works:** Models grade their own work generously, inherit their own assumptions, and frequently interpret “a code path exists” as “the feature works.” Independent evaluation produces different failure hypotheses.

A recent shipped detective-game prototype used:

- Writer agents for cases, characters, dialogue, and deduction logic.
- Verifier agents checking solvability, clue consistency, contradictions, and accidental solution leakage.
- An automated playtester that clicked through the actual game, captured screens, and reported friction and visual mismatches.
- A repeated generate → critique → fix → playtest loop. ([Reddit][7])

Other recent builders reported similar patterns: adversarial review after each implementation step, Claude and Codex checking one another, or a separate planning conversation being stress-tested against the existing codebase before implementation. ([Reddit][8])

The checker should ideally have:

- A different prompt and role.
- A clean context.
- Read-only access when possible.
- The original acceptance criteria.
- Permission to reject completion.
- Sometimes a different model.

---

## Golden Rule 6: Promote repeated work out of the agent loop

**The rule:** Use deterministic code for anything that does not require model judgment.

**Why it works:** Agents are expensive, probabilistic workflow engines. Parsing, formatting, file movement, schema validation, counters, asset conversion, and build invocation are better expressed as scripts.

One r/aigamedev contributor suggested that when an agent has been asked to perform the same operation three times, it is worth considering a script. Another described custom tooling that exports Blender scenes directly into Unity scenes and prefabs while converting materials automatically. ([Reddit][9])

A useful routing hierarchy from recent long-running-agent discussion is:

1. **Deterministic code:** parsing, validation, deduplication, file operations, schema checks.
2. **Cheap model:** extraction, formatting, classification, tool-output summarization.
3. **Strong model:** architecture, ambiguous planning, cross-system reasoning.
4. **Human gate:** destructive operations, production changes, credentials, money, or irreversible design decisions. ([Reddit][10])

Useful game-dev harness components include:

- Headless engine commands.
- Scenario-generation scripts.
- Save-state validators.
- Screenshot and visual-diff runners.
- Performance benchmark scenes.
- Asset import/export pipelines.
- Hooks that require a build or smoke test before the agent can finish.
- JSON-schema validation at every agent handoff.

---

## Golden Rule 7: MCP is a capability adapter, not a default requirement

**The rule:** Add an MCP server only when it provides a concrete capability that the CLI, filesystem, or existing API cannot provide efficiently.

**Why it works:** Every MCP tool definition consumes context and adds another routing decision. Recent r/mcp discussion reported that once several similarly described tools are present, agents increasingly choose the wrong tool or construct incorrect parameters. ([Reddit][11])

A recent Godot comparison reported:

- MCP run: approximately 27 minutes and 11% of the user’s weekly allocation.
- Headless-Godot run: approximately three minutes and 2%.
- The non-MCP result was judged visually better by the poster. ([Reddit][12])

A June X prototype builder similarly reported that Unreal’s experimental MCP integration slowed the game-building loop rather than improving it. ([X (formerly Twitter)][13])

But the evidence is not “MCP is bad.” Another builder reported successfully reconstructing a childhood 3D game after giving the agent Godot and Blender MCP capabilities. ([X (formerly Twitter)][14])

The practical rule is therefore:

- Use CLI/headless execution for code, tests, builds, and ordinary file operations.
- Use MCP for capabilities such as manipulating a visual scene, controlling Blender, gathering engine-state unavailable from files, or interacting with an editor.
- Benchmark the MCP path against the simpler path on **quality, tokens, latency, reliability, and recovery**.
- Expose only the tools needed for the current task.
- Make tool names and schemas visibly distinct.
- Prefer task-specific MCP profiles over one enormous configuration.
- Grant the narrowest permissions possible.

---

## Golden Rule 8: Bound autonomy outside the model

**The rule:** Wall-clock, step, retry, spend, filesystem, and resource limits must be enforced by the process running the agent—not merely requested in the prompt.

**Why it works:** Routing to cheaper models reduces the cost of each call but does not limit how many calls an agent can make. Recent practitioners recommend an externally enforced envelope containing a maximum runtime, maximum step count, spend cap, retry cap, and whitelist of writable resources. ([Reddit][10])

This also prevents resource coordination failures. One X game-building experiment woke to a crashed Mac after multiple agents opened the game simultaneously to capture screenshots. ([X (formerly Twitter)][13])

Useful safeguards include:

- Maximum iterations.
- Maximum consecutive attempts without a changed test result.
- Hard token or dollar ceiling.
- Maximum tool retries.
- One agent lease per editor, GPU, or game instance.
- Writable-path allowlist.
- No destructive action without a human gate.
- Git worktree or branch per worker.
- Known-good, version-pinned environment image.
- Automatic checkpoint before migrations and broad refactors.
- Kill-and-escalate behavior rather than indefinite replanning.

Persistent environments retain warm indexes and task state, but recent builders identify environment drift as their central unresolved tradeoff; preinstalled, known-good images and version locking reduce that risk. ([Reddit][15])

---

## The recurring failure modes

### Agentic laziness

The agent declares success after partial completion: a class exists, one probe passed, or the API returned HTTP 200. Recent X discussion explicitly identifies premature “done” declarations as a long-running-agent failure mode. ([X (formerly Twitter)][16])

**Prevention:** outcome contracts, persistent acceptance tests, and a checker empowered to reject completion.

### Goal drift

The implementation remains productive but moves toward the wrong product. A recent GTA-like agent project famously created the wrong city while still adding visible functionality. ([X (formerly Twitter)][13])

**Prevention:** explicit invariants, reference images, small subphases, durable decisions, and human design gates.

### Self-confirming QA

The same context writes the code, creates temporary probes, interprets the results, and marks its own checklist complete.

**Prevention:** tests authored first, separate test ownership, clean-context reviewers, and external metrics. ([Reddit][4])

### Context accretion

The agent repeatedly carries logs, tool responses, old plans, and stale assumptions forward until the useful signal becomes difficult to recover.

**Prevention:** compact structured state, logs stored separately, selective retrieval, and task-scoped contexts. ([Reddit][10])

### Tool-surface explosion

More tools create more opportunities for incorrect routing, parameter confusion, context consumption, permissions problems, and accidental concurrency.

**Prevention:** expose tools per task, promote common operations to scripts, and benchmark MCP against simpler interfaces. ([Reddit][11])

### Architecture collapse under rapid expansion

The agent can add dozens of features before anyone notices that the simulation, dependency graph, or data model is unsustainable.

**Prevention:** systems-first design, modular boundaries, dependency-ordered roadmap, TDD, performance budgets, and frequent playable checkpoints. ([Reddit][17])

---

## A minimal high-reliability game-dev harness

Based on the repeated advice, a strong starting scaffold would be:

```text
/docs
  DESIGN.md          Systems, interfaces, invariants
  ROADMAP.md         Dependency-ordered, session-sized subphases
  DECISIONS.md       Important choices and rationale

/agent
  STATE.md           Current facts, progress, blockers, next task
  RULES.md           Small set of non-discoverable hazards
  eval-rubric.json   Completion and escalation rules

/tests
  acceptance/        Written before implementation
  scenarios/         Small deterministic gameplay scenes
  performance/       Frame-time and simulation budgets
  visual/            References and screenshot comparisons

/tools
  build
  smoke-test
  run-scenario
  validate-save
  benchmark
  capture-screenshot
  summarize-results
```

The operating loop:

```text
Planner selects one ready task
        ↓
Maker implements in an isolated worktree
        ↓
Scripts build, lint, test, and benchmark
        ↓
Independent checker evaluates acceptance criteria
        ↓
Automated player exercises the actual build
        ↓
Human reviews game-feel or design decisions
        ↓
Commit + update durable state, or roll back
```

Use an engine MCP or Blender MCP only for the portions that genuinely require editor or scene-level control. Keep routine development on filesystem, CLI, headless engine, tests, and scripts. The agent should never be able to redefine success after seeing its implementation.

**The most important meta-rule:** autonomy should increase only after the verification and recovery paths have already proven reliable.

---

## Research note

X indexing exposes posts unevenly and often omits complete reply trees, so the highest-confidence conclusions are those independently repeated across Reddit threads or paired with a public prototype and concrete failure report.

---

## Citations

[1]: https://www.reddit.com/r/aigamedev/comments/1v8za8g/how_are_you_guys_making_good_games_quickly/ "How are you guys making good games quickly? : r/aigamedev"

[2]: https://x.com/addyosmani/highlights "Addy Osmani (@addyosmani) / Highlights / X"

[3]: https://x.com/nykdotdev/status/2078301264220405977 "Before adding another line to CLAUDE.md, ask where the ..."

[4]: https://www.reddit.com/r/aigamedev/comments/1va3loa/how_one_mvp_turned_into_27_systems/ "How One MVP Turned Into 27 Systems : r/aigamedev"

[5]: https://www.reddit.com/r/AI_Agents/comments/1v26o33/how_do_you_stop_agents_from_reporting_success/ "How do you stop agents from reporting success after a tool only partially worked? : r/AI_Agents"

[6]: https://www.reddit.com/r/ClaudeCode/comments/1v3klwm/goal_loop_and_four_ways_to_loop_an_agent_in/ "/goal, /loop, and Four Ways to Loop an Agent in Claude Code : r/ClaudeCode"

[7]: https://www.reddit.com/r/vibecoding/comments/1tzgwj5/how_far_can_ai_agents_actually_go_in_making_a/ "How far can AI agents actually go in making a game? … prototype. : r/vibecoding"

[8]: https://www.reddit.com/r/aigamedev/comments/1ulr6t2/how_are_you_using_ai/ "How are you using AI? : r/aigamedev"

[9]: https://www.reddit.com/r/aigamedev/comments/1u1b7tq/what_ai_are_you_guys_using_to_develop_your_game/ "What Ai are you guys using to develop your Game? : r/aigamedev"

[10]: https://www.reddit.com/r/AI_Agents/comments/1v9ckav/how_are_people_keeping_longrunning_ai_agent_costs/ "How are people keeping long-running AI agent costs under control? : r/AI_Agents"

[11]: https://www.reddit.com/r/mcp/comments/1v9n747/are_people_actually_using_mcp_for_what/ "Are people actually using MCP? For what? : r/mcp"

[12]: https://www.reddit.com/r/aigamedev/comments/1v484mi/mcp_is_worse_than_no_mcp_godot/ "MCP is worse than no MCP (Godot) : r/aigamedev"

[13]: https://x.com/ziwenxu_/status/2067317975992979476 "Ziwen on X: I let a loop of AI agents build GTA 6 while I ..."

[14]: https://x.com/Stefan_3D_AI/status/2080648445145321539 "Stefan 3D AI (@Stefan_3D_AI) on X"

[15]: https://www.reddit.com/r/AI_Agents/comments/1v48y2l/persistent_machines_vs_ephemeral_sandboxes_for/ "Persistent machines vs ephemeral sandboxes for long-running agents : r/AI_Agents"

[16]: https://x.com/seeconvm/status/2081041999004606581 "seeco (@seeconvm) on X"

[17]: https://www.reddit.com/r/aigamedev/comments/1v414b6/fable_built_this_prototype_in_a_few_hours/ "Fable built this prototype in a few hours. : r/aigamedev"
