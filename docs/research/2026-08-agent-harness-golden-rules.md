# Golden Rules: Agent Harnesses, Agentic Loops & Evaluation

- **Window:** June – early August 2026  
- **Focus:** robust harnesses/scaffolding, MCP + skills tooling, agentic loops, success criteria/eval, long-running failure modes  
- **Communities:** r/aigamedev, r/LocalLLaMA (primary architecture debate), X practitioners  
- **Sources:** [2026-08-sources.md](2026-08-sources.md)  
- **Confidence overall:** High on harness/context principles; Medium on exact product rankings (churn is fast)

---

## Thesis (repeated everywhere)

**The harness is the product.** For the same model, harness choice moves cost ~2× (Databricks internal eval, Jul 2026) and quality more than raw model “IQ.” Game demos that shipped in this window (e.g. long-run Three.js / “GTA-like” prototypes) explicitly credit **loop + feedback harness**, not the prompt.

A harness is the *deterministic software* around the LLM: tool loop, context curation, permissions, skills loading, session/state, audit, stop conditions. The model thinks; the harness steers.

---

## Golden Rules

### 1. Build the harness (and feedback) before the feature content

**Rule:** For game or long-horizon work, instrument success *before* generating levels, systems, or assets.

**Why it works:** Agents cannot “see” gameplay the way humans do. Without numbers and assertions, the loop optimizes vibes and plateaus on a broken 3D world. Shippers who recovered from failed first loops did so by adding telemetry and tests, not by writing longer prompts.

**Patterns / tools (shipped demos):**
- r/aigamedev GTA experiment (smith2008 + Gauntlet Loop, Aug 2026):  
  - JSON telemetry (frame times, p99, draw calls, heap, physics cost)  
  - Determinism aids: fixed timestep, seeded RNG, recorded inputs  
  - Playwright scenarios with *gameplay* assertions (no fall-through, reached waypoint)  
  - Golden-frame pixel diffs  
  - Contact sheets + **narrow** visual rubrics (explicit failure modes, not “does it look good”)  
  - Real GPU (headed Chrome / Vulkan) — headless SwiftShader lied about perf  
  - **Structured game-state JSON ≫ video/frames alone** for Claude Code feedback  
- 22h / 86 agents was viable only after multiple loop + harness iterations

**Confidence:** High (game shippers + long-horizon research align)

---

### 2. Lean context beats kitchen-sink prompts

**Rule:** Minimize always-on system prompt + tool schemas. Prefer small base harnesses; load depth on demand.

**Why it works:** Bloated context is the silent killer of reliability (Anthropic-aligned discourse; repeated on LocalLLaMA). Attention dilutes; original constraints get buried; cost rises. Databricks: leaner harness → ~3× less context/turn → large cost win at same model. Local users report Pi-class harnesses (~2k system tokens, 4 tools) outperform feature-heavy stacks that compact constantly.

**Patterns / tools:**
- **Pi** (pi-mono / pi.dev): read/write/edit/bash; packages opt-in; community “Arch of harnesses”
- **little-coder**, **oh-my-pi**, **late-cli**: isolated task agents / local-model hardening
- Explicit warning: every Pi package / MCP you add re-bloates the prompt — then you might as well use Claude Code
- “Specific prompts + reducing turns = more accurate + less tokens + faster” (LocalLLaMA, production local agents)

**Confidence:** High

---

### 3. Skills for behavior; MCP (or gateways) for systems — not everything is a tool

**Rule:** Use **Skills** (markdown playbooks + scripts, lazy-loaded) for “how we do X.” Use **MCP / tools** only for true external systems. Prefer few tools with rich composition over dozens of always-loaded schemas.

**Why it works:** MCP schemas sit in context all session (token tax). Skills load when triggered. Reddit consensus mid-2026: “make those skills instead; MCP servers eat context.” X: MCP = connection layer; Skills = operating layer; best setups use both.

**Patterns / tools:**
- Agent Skills vs MCP (Femke Plantinga, Jul 2026; Andrew Ng / Anthropic course discussion)
- Hermes: auto-write skills for repeatable tasks; disable junk built-in skills
- Complex greenfield apps: **REPL-first / code-execution against specs** instead of MCP explosion (`@neural_avb`, Aug 2026) — agent writes code against serializable contracts; skills teach the data model
- Production alternative to MCP sprawl: single **gateway API**, large payloads as **files**, **per-turn agent tokens**, capability grants at call time (`@ai_ops_lead`)
- Local MCP that stays popular: filesystem, Playwright, **local SearXNG** (not 20 cloud MCPs)

**Confidence:** High

---

### 4. Explicit durable state; treat the context window as ephemeral

**Rule:** After every meaningful action, write state to files (or DB). Before acting, re-read state. Never rely on the chat transcript as the only memory.

**Why it works:** Long runs hit context rot, attention U-shape (middle ignored), context collapse (conflicting tool outputs), and accumulated noise (arXiv 2607.14275 discourse). Mid-trajectory compaction is *not* a free fix — some ultra-long software benchmarks saw **0/71** compacted rollouts pass vs non-zero without. Behavioral state decay (Meta): facts/subgoals buried → active memory injection helps more than passive RAG.

**Patterns / tools:**
- State file / PR checkpoint pattern (multi-thousand sessions)
- End-of-session summaries + morning startup routines (LocalLLaMA personal agents)
- Redis/Postgres (or simpler markdown state) for tasks + skills learned
- Meta-style **memory agent** alongside action agent (inject reminders; stay silent when unneeded)
- Sub-agents with **fresh context** for web search / noisy work so main context stays clean

**Confidence:** High

---

### 5. Define machine-checkable success criteria — then loop until green

**Rule:** Success is not “agent says done.” Success is tests green, metrics threshold, assertions pass, screenshots match, or rubric scores. The loop is: **act → measure → keep/revert → repeat** until criteria or budget.

**Why it works:** Loops optimize whatever signal you give them (muratcan / self-improvement skill: “just let the agent improve itself” is the wrong model). AutoLab: long-horizon winners are models that **persist** — re-benchmark, edit, fold empirical feedback under a wall-clock budget — not one-shot quality. Game loops: Playwright + golden frames + telemetry give the same empirical spine.

**Patterns / tools:**
- Anthropic patterns: single agent loop (80% of cases); sequential; parallel; hierarchical; **evaluator-optimizer** (2–4 critique cycles)
- **Gauntlet Loop** / multi-sprint “don’t stop until all items green”
- Ralph-style / autoresearch: change code → run metric → retain/revert
- Custom SWE-bench on *your* repo (Superconductor discourse; Databricks internal tasks)
- OpenCode “failsafes” for long unattended runs until task complete

**Confidence:** High

---

### 6. Start single-agent and minimal; multi-agent only with isolation and handoffs

**Rule:** Default to one agent in a tight tool loop. Split into specialists when context pressure or role conflict appears — and give each sub-agent **narrow tools, own context, and a written handoff**.

**Why it works:** Multi-agent can win on hard tasks (Anthropic multi-agent claims in circulating decks) but multi-agent *noise* is a top complaint: token burn, thrash, hard to debug. LocalLLaMA: specialized hand-crafted system prompts for sub-agents beat one kitchen-sink personality. Isolation (task agents with separate context) is the main win of late-cli and similar.

**Patterns / tools:**
- Orchestrator (plan) + cheap workers (edit)
- Sub-agents for startup/shutdown/research so main session keeps budget
- Hermes orchestrating OpenCode as coding worker
- “Scripts calling LLMs” sometimes beats “LLM calling every script” for multi-step work

**Confidence:** High on “minimal first”; Medium on multi-agent magnitude claims

---

### 7. Permissions, intent checks, and action receipts — not prompt-only safety

**Rule:** Tool access is policy at runtime: scopes, sandboxes, approval for risky ops, and a chronological audit trail. Skills from the internet are supply-chain risk.

**Why it works:** Agents with “permission in the prompt” still exfiltrate or `rm -rf`. Community hardening: VM/workspace restrict, no network or egress allowlist, no live secrets in agent-readable compose files, pin deps (Hermes supply-chain scare). Intaris-class MCP proxy: check proposed call against **user intent**, not only allowlist. Action receipts UI: agent → intent → tool → args → decision → result beats another chat pane.

**Patterns / tools:**
- Intaris (MCP/tool proxy + guardrails)
- Per-skill permission scoping; runtime monitoring (Moltwire / arc-gate discourse)
- Workspace-only FS; separate UID + firewall for agent processes
- Read skill files before install; treat skills as untrusted code

**Confidence:** High

---

### 8. Engineer the loop types deliberately (loop engineering)

**Rule:** Don’t only “while tools: call tools.” Choose loop shapes for the job and budget.

**Common shapes (window consensus):**

| Loop | Use when | Stop when |
|------|----------|-----------|
| Single tool loop | Most coding/tasks | No more tool calls / done signal |
| Sequential workflow | Auditable pipelines | Last stage passes |
| Parallel fan-out | Independent research/gen | Merge + conflict resolution |
| Hierarchical supervisor | Multi-skill projects | Supervisor accepts deliverable |
| Evaluator-optimizer | Quality bar needed | Score ≥ threshold or N rounds |
| Self-improve / meta-harness | Scaffolding itself is the product | Guardrailed metric only — never unbounded self-rewrite |

**Why it works:** Google/Anthropic course traffic in Jul–Aug 2026 centers “loop engineering” and “graph engineering.” Long unattended systems need explicit stop, budget, and human escalate points (HITL).

**Tools / names:** Gauntlet Loop; DeerFlow-class super harness (subagents + memory + sandboxes + skills); Anthropic Building Effective Agents PDF circulating on X.

**Confidence:** High on taxonomy; Medium on any single product

---

### 9. Evaluate the harness on *your* tasks — and instrument failure proxies

**Rule:** Public leaderboards are secondary. Run evals on your codebase, game scenarios, and infra. Log stop reasons, retries, files touched, apologies/drift signals.

**Why it works:** Same model, different harness → different cost/quality (Databricks). Agents can game verifiers; watch for reward hacks. Cheap proxy: rising “sorry / you’re absolutely right” rate → context/docs problem, not model problem (Arpit Bhayani). Context-quality scoring before action (ProofAgent-Harness discourse) shifts instrumentation from “was the action right?” to “was the context worth acting on?”

**Patterns:**
- Chronological run logs (commands, failed tools, stop reason)
- Custom coding evals across MCP/tool combinations (LocalLLaMA eval authors)
- Apology / contradiction counters
- Persistence metrics under wall-clock budget (AutoLab-style)

**Confidence:** High

---

### 10. Prefer reconstructable boring harnesses over magical multi-agent demos

**Rule:** Optimize for **replay and debug**. If you cannot reconstruct why a file changed, the system will not scale to long runs or multi-agent ownership.

**Why it works:** When local models or long loops mis-edit, the winners are people who can inspect tool args, context at failure, and deterministic repro. “Boring harness + deterministic helpers for repo map/tests” beat flashy multi-agent on LocalLLaMA mid-2026.

**Patterns:**
- Continual traces (logs that accumulate structure, not just printf)
- `/tree` or branch-and-retry conversation forking (Pi)
- Spec-driven modules with serializable contracts
- Hashline / better edit tools when search-replace fails on local models

**Confidence:** High

---

## Failure modes → mitigations (cheat sheet)

| Failure mode | Symptom | Mitigation (what people who ship do) |
|--------------|---------|--------------------------------------|
| **Context rot / noise** | Silent quality drop; ignores early constraints | Prune intermediates; re-ground to goals every N turns; file-backed large tool outputs |
| **Attention U-shape** | Important mid-context ignored | Keep critical state at ends; short windows; active memory inject |
| **Context collapse** | Conflicting tools → silently wrong | Multi-juror / verify context; cite sources; don’t average blindly |
| **Behavioral state decay** | Forgets decisions/subgoals | Explicit state files; memory agent; checklists |
| **Bad compaction** | Compaction correlates with failed long rollouts | Prefer fresh sub-agents + durable state over endless summarize |
| **MCP / tool bloat** | Slow, confused, expensive | Skills-first; few tools; gateway; lazy load |
| **Edit thrash (local models)** | Wrong search-replace; sed loops | Better edit tools (hashline); grammar-constrained tools; smaller tool surface |
| **Unbounded autonomy** | Token fire, drift, unsafe actions | Budgets, HITL gates, action receipts, sandbox |
| **Skill supply chain** | Prompt injection via markdown skills | Review skills; runtime policy by instruction source; pin versions |
| **Vibe-only game feedback** | Pretty but broken gameplay | Telemetry + Playwright + state JSON + golden frames |
| **Harness gaming evals** | High score, bad real task | Private task suites; human spot-check; multiple verifiers |
| **Over-multi-agent** | Noise, cost, un-debugable | Single agent until proven need; isolate contexts |

---

## Practical stack patterns named by people who shipped

### Coding / general agent harness
- **Minimal path:** Pi (+ selective packages) or OpenCode with failsafes  
- **Batteries / multi-channel:** Hermes, OpenClaw (Pi-based remote access)  
- **Frontier hosted:** Claude Code / Codex — still baseline for hard refactors  
- **Standardizing:** Goose (AAIF discourse)

### Game / long creative loops
- **Gauntlet Loop** + custom harness: telemetry, determinism hooks, Playwright, golden frames, contact sheets, **game-state JSON exporters**
- Review → plan → sprint list → loop until green  
- Prefer browser/WebGPU stacks that agents can drive and measure

### MCP / skills layout (recommended default)
```
Always-on:  read, edit/write, bash, (optional) search
On-demand skills:  domain playbooks (godot, art pipeline, release)
MCP only when needed:  engine control, browser, art APIs, issue tracker
Never:  15 MCP servers with full schemas every turn
```

### Eval skeleton
1. Fixed task suite (your game smoke tests or repo PRs)  
2. Machine metrics + optional narrow LLM rubric  
3. Log: tools, tokens, stop reason, apologies, human overrides  
4. Compare harness A vs B at **same model** before changing models  

---

## Implications for FI-Harness

If this repo becomes a game-dev agent harness, community evidence says prioritize in this order:

1. **Success criteria + runners** (play tests, golden frames, state dumps, unit/smoke)  
2. **Lean core loop** + durable project state on disk  
3. **Skills library** for game workflows; MCP only for real external systems  
4. **Action receipts + budgets + sandbox**  
5. Multi-agent / self-improving harness last — and only against a frozen metric  

---

## Open questions (for later research)

- Best default edit-tool semantics for mid-size local coders (hashline vs range vs AST)?  
- When does REPL/code-act beat MCP for engine control (Godot/Unity)?  
- Reliable mid-horizon compaction that doesn’t kill pass rate?  
- Minimal game-state schema that generalizes across genres for agent feedback?

---

*Digest owner: research agent pass 2026-08-03. Next agent: please add dated notes rather than rewriting this file wholesale; append “Amendments” if needed.*
