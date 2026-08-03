# Sources — Agent harness research (Jun–early Aug 2026)

Compiled 2026-08-03. Focus: r/aigamedev, r/LocalLLaMA (primary high-signal agent/harness discussion), and X.

## Reddit

| Thread | Sub | Approx | Why it matters |
|--------|-----|--------|----------------|
| [Agent and harness development](https://old.reddit.com/r/LocalLLaMA/comments/1u7aqyd/agent_and_harness_development/) | r/LocalLLaMA | mid Jun 2026 | How to learn harnesses: tiny loop + tools + eval; Anthropic “Building Effective Agents”; open harness dissection |
| [Are harnesses like OpenClaw and Hermes really necessary?](https://old.reddit.com/r/LocalLLaMA/comments/1tbgarl/are_harnesses_like_openclaw_and_hermes_really/) | r/LocalLLaMA | May–Jun 2026 (active into window) | Layers beyond chat: tool boundaries, intent checks, action receipts, specialized sub-agents, context budgeting |
| [favorite Agentic Coding Harness](https://old.reddit.com/r/LocalLLaMA/comments/1th5t1b/favorite_agentic_coding_harness/) | r/LocalLLaMA | ~May–Jun 2026 | Pi vs OpenCode vs Hermes; skills > always-on MCP; lean context wins; reconstructable runs |
| [What exactly does Pi harness mean?](https://old.reddit.com/r/LocalLLaMA/comments/1t0fg3y/what_exactly_does_pi_harness_mean/) | r/LocalLLaMA | ~May 2026 | Community definition of harness; context/tool/skill/workflow responsibilities |
| [GTA 6 first attempt… right harness and agentic loops](https://old.reddit.com/r/aigamedev/comments/1ve7xt5/gta_6_first_attempt_far_from_perfect_but_its/) | r/aigamedev | early Aug 2026 | Shipped prototype: Gauntlet Loop + telemetry, determinism, Playwright, golden frames, structured game-state JSON |
| [The supply chain problem… agent skill files](https://old.reddit.com/r/LocalLLaMA/comments/1rgelk1/the_supply_chain_problem_nobody_talks_about_agent/) | r/LocalLLaMA | earlier but still cited | Skill supply-chain risk; runtime governance |

Note: r/aigamedev in this window is demo-heavy; the deepest *harness architecture* debate lives in r/LocalLLaMA + X, with game-specific loop engineering concentrated in a few aigamedev ship posts (e.g. smith2008 GTA experiment).

## X (selected high-signal)

| Author / post | Date | Topic |
|---------------|------|--------|
| @femke_plantinga — 6(+1) architectural pillars (MCP, loops, skills, multi-agent, agentic RAG, memory, HITL) | 2026-06-25 | Stack map |
| @femke_plantinga — MCP vs Skills | 2026-07-16 | Connection layer vs operating layer |
| @alighodsi (Databricks) — same model, different harness ≈ 2× cost | 2026-07-08 | Harness > model for cost/quality |
| @muratcan — self-improvement loops / meta-harnesses; optimize the signal you give | 2026-07-08 | Loop design caution |
| @AiCamila_ — Agent Harness cheatsheet | 2026-07-28 | Context, permissions, sessions, lifecycle |
| @neural_avb — REPL-first / spec-driven over MCP sprawl for complex apps | 2026-08-02 | Tool surface design |
| @ai_ops_lead — gateway API, file-backed large responses, per-turn tokens | 2026-08-02 | Production MCP alternatives |
| @mrru5s3ll — silent context degradation in long runs | 2026-08-02 | Context rot failure mode |
| @stretchcloud — “Agents do not fail alone: the context fails first” (arXiv 2607.14275) | 2026-07-28 | Context quality eval |
| @omarsar0 / DAIR — Meta memory agent; behavioral state decay | 2026-07-10 | Active memory injection |
| @arpit_bhayani — “apology metric” as context quality signal | 2026-07-10 | Cheap eval proxy |
| @johniosifov — explicit state on disk, context ephemeral | 2026-08-02 | Long-horizon state |
| @dair_ai — AutoLab / persistence under wall-clock budget | 2026-06-04 | Long-horizon persistence |
| @HsineGh — Copilot research: tools + loops > raw model IQ | 2026-07-28 | Harness primacy |

## Papers / named systems (mentioned in window)

- Anthropic *Building Effective Agents* (patterns: single loop, sequential, parallel, hierarchical, evaluator-optimizer)
- arXiv 2607.14275 — context failure modes (rot, attention U-shape, collapse, noise)
- Meta work on behavioral state decay + active memory agent (Terminal-Bench / tau-bench lifts)
- AutoLab — long-horizon persistence under budget
- Gauntlet Loop (Matt Shumer) — used in aigamedev long-run game builds
- Harnesses named by practitioners: **Pi** (pi-mono / pi.dev), **OpenCode**, **Hermes**, **OpenClaw**, **Claude Code**, **Codex**, **Goose**, **little-coder**, **oh-my-pi**
- Patterns: **Ralph / open-ralph** style fix-test loops; **karpathy/autoresearch**; Intaris-style MCP proxy/guardrails

## Confidence legend (used in golden rules)

- **High** — repeated independently across Reddit + X + at least one shipper demo or corporate eval
- **Medium** — strong consensus in one community, lighter corroboration elsewhere
- **Low** — insightful but single-source or early
