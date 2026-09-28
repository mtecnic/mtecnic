<div align="center">

```
  ███╗   ███╗████████╗███████╗ ██████╗███╗   ██╗██╗ ██████╗
  ████╗ ████║╚══██╔══╝██╔════╝██╔════╝████╗  ██║██║██╔════╝
  ██╔████╔██║   ██║   █████╗  ██║     ██╔██╗ ██║██║██║
  ██║╚██╔╝██║   ██║   ██╔══╝  ██║     ██║╚██╗██║██║██║
  ██║ ╚═╝ ██║   ██║   ███████╗╚██████╗██║ ╚████║██║╚██████╗
  ╚═╝     ╚═╝   ╚═╝   ╚══════╝ ╚═════╝╚═╝  ╚═══╝╚═╝ ╚═════╝
```

### A self-hosted AI toolbox. Truly measured. Some vibes.

**Tools for people running AI on hardware they own** — serve it, measure it, shrink it, build with it, drive it.

No cloud. No API keys. No rented GPUs. Every number below came off a machine in my house.

![Python](https://img.shields.io/badge/python-3.12+-3776AB?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/typescript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)
![vLLM](https://img.shields.io/badge/serving-vLLM-orange?style=flat-square)
![Hardware](https://img.shields.io/badge/11%C3%97RTX%203090%20%C2%B7%205090%20%C2%B7%20RTX%20PRO%205000%20%C2%B7%202%C3%97DGX%20Spark-76B900?style=flat-square&logo=nvidia&logoColor=white)
![Cloud](https://img.shields.io/badge/cloud-none-2ea44f?style=flat-square)

</div>

---

Two threads run through everything here: **agents that ship finished, validated software** — not snippets — and **making big models cheaper to run** on hardware you can actually buy. Around both sits the unglamorous layer nobody writes: the thermal logging, the fan curves, the launch topology, the session dashboards. Results get published either way, including the ones that didn't work.

**The lab:** 2× DGX Spark · two 4× RTX 3090 boxes · a third box with 3× RTX 3090 + an RTX PRO 5000 Blackwell (72 GB) · an RTX 5090. Eleven 3090s is how you end up publishing benchmarks about *interconnect topology* instead of guessing about it — and why the thermal and fan-curve tooling below exists at all.

---

## 🚀 Shipping this week

Four tools coming out of the private pile. All of it is the stuff you only build after running a home lab long enough to get burned by it.

| Repo | What it does |
|---|---|
| [**nvidiacp**](https://github.com/mtecnic/nvidiacp) | Full NVIDIA control panel for **headless** Linux — power limits, app clocks, ECC, MIG, compute mode, persistence. **Fan control without X** (no coolbits), a closed-loop temperature-curve fan daemon, and a **vLLM launch optimizer** that sizes tensor-parallel plans to your actual VRAM across 5 workload profiles. Settings survive reboots via systemd. Pure stdlib Python + vendored NVML — **zero pip installs**. |
| [**tempmon**](https://github.com/mtecnic/tempmon) | Thermal watchdog for inference boxes, with the metric `nvidia-smi` **can't give you**: per-card **GDDR6X VRAM junction temperature** — the thing that actually cooks a 3090 (throttles ~105°C) and returns `N/A` on GeForce. Crash-proof by design: every cycle is `flush()`ed and `fsync()`ed, so when the box thermally hard-locks you still have proof of how hot it got. |
| [**ramp**](https://github.com/mtecnic/ramp) | *Real Agents Maximizing Productivity* — a local-first coding agent harness, built against a LAN of vLLM boxes and hardened by the runs that went wrong. See [the cockpit](#️-the-cockpit) below. |
| [**muxter**](https://github.com/mtecnic/muxter) | Every tmux session on the machine, in one terminal, live. Read-only by default so you can watch a long build or an agent mid-task without risking a keystroke; press `i` to step in, `Esc` to step back out. Attaches as an ordinary tmux client — nothing to install on the sessions. Check an agent fleet from a phone SSH app. |

## 🔌 Run the lab

Serving, benchmarking, and knowing what your hardware is actually doing.

| Repo | What it does | ★ |
|---|---|---|
| [**vllm-topology-bench**](https://github.com/mtecnic/vllm-topology-bench) | **Replicas > tensor parallelism.** Everyone reaches for `--tensor-parallel-size 4` to "use all the GPUs." On no-NVLink 3090s that's the *slowest* feasible option: two `TP=2` replicas beat one `TP=4` at **every** concurrency — **+136% throughput at 128 requests** (1,437 vs 610 tok/s) and **TTFT 1.4 s vs 3.9 s**. And "one copy per card" doesn't even fit — `4× TP=1` OOMs. Reproducible harness included. | ![★](https://img.shields.io/github/stars/mtecnic/vllm-topology-bench?style=flat-square&label=★) |
| [**model-chat-cli**](https://github.com/mtecnic/model-chat-cli) | Terminal command center for local AI servers. **Auto-discovers Ollama, LM Studio, and vLLM on your LAN** — no config. Chat with real TTFT and decode metrics, run blind multi-model battles with auto-judged tournaments, load-test to saturation, and score models on a **45-task agentic benchmark** across 6 difficulty tiers. | ![★](https://img.shields.io/github/stars/mtecnic/model-chat-cli?style=flat-square&label=★) |

## 📉 Make big models cheaper

Smaller models, fewer tokens, same behavior — with the measurements to prove it.

| Repo | What it does | ★ |
|---|---|---|
| [**tokopt**](https://github.com/mtecnic/tokopt) | Post-hoc BPE tokenizer adaptation for **already-deployed** LLMs — script-aware pruning, continued BPE extension, embedding-only calibration. **−4.9% bits/char, −4.2% tokens, −18% INT4 model size (4.4 → 3.6 GB)**, HumanEval within noise, greedy generation byte-identical. Reproduces in **~3 hours on a single RTX 3090**. Ships a ~9,000-word PAPER.md and a 13-section REPRODUCE.md. | ![★](https://img.shields.io/github/stars/mtecnic/tokopt?style=flat-square&label=★) |
| [**research-test-Qwen3-Coder-Next-REAP-AWQ**](https://github.com/mtecnic/research-test-Qwen3-Coder-Next-REAP-AWQ) | REAP expert pruning + AWQ on a **149 GB BF16 MoE** coder model. **512 → 410 experts per layer** via 4-dataset saliency calibration with super-expert preservation, then W4A16 @ group_size 32 — to get a frontier-size coder onto consumer GPUs. | ![★](https://img.shields.io/github/stars/mtecnic/research-test-Qwen3-Coder-Next-REAP-AWQ?style=flat-square&label=★) |
| [**ctx**](https://github.com/mtecnic/ctx) | **CTX (Context Transfer Format)** — an interchange format for LLM web consumption. A 1.2 MB Wikipedia page becomes 150 KB of structure-preserving CTX: **−87% bytes, ~90% fewer tokens**, citations and hierarchy intact. CLI, Python library, and a FastAPI service with Redis caching + transparent proxy. | ![★](https://img.shields.io/github/stars/mtecnic/ctx?style=flat-square&label=★) |

## 🤖 Agents that finish

Autonomy is easy. *Finished* is the hard part.

| Repo | What it does | ★ |
|---|---|---|
| [**cadillac**](https://github.com/mtecnic/cadillac) | Autonomous coding agent: a sentence in, a validated app out. 12-stage pipeline (SPEC → … → CRITIC → RUNTIME → PACKAGE) with operational gates, a completeness CRITIC, runtime flow verification, and surgical-mode stuck-loop recovery. Works against **any** OpenAI-compatible endpoint. **616 tests · 146 apps built · 403 lessons accumulated.** | ![★](https://img.shields.io/github/stars/mtecnic/cadillac?style=flat-square&label=★) |
| [**cadillac-builds**](https://github.com/mtecnic/cadillac-builds) | The receipts: **26 runnable applications built unattended with zero human edits**, published from a scan of ~99 build workspaces. Every project's README lists exactly which validation checks passed — **and which didn't**. | ![★](https://img.shields.io/github/stars/mtecnic/cadillac-builds?style=flat-square&label=★) |
| [**graphx**](https://github.com/mtecnic/graphx) | Describe a pipeline in English, get a real agentic workflow, run it in your terminal — all on your own model. Pregel-style supersteps, cyclic graphs with loops, per-step SQLite checkpointing (kill a run, `resume` continues), per-node retries + model fallback chains + budgets, human approval gates, **12 credential-wired connectors**, and `secret://` refs that never reach logs or checkpoints. **298 tests.** | ![★](https://img.shields.io/github/stars/mtecnic/graphx?style=flat-square&label=★) |
| [**workerAI**](https://github.com/mtecnic/workerAI) | ReAct-style autonomous agent on vLLM + LangGraph — plans, reasons, and executes multi-step tasks against a library of specialized tools. | ![★](https://img.shields.io/github/stars/mtecnic/workerAI?style=flat-square&label=★) |

## 🖥️ The cockpit

Driving a multi-node lab from one chair.

| Repo | What it does | ★ |
|---|---|---|
| [**clusterspace**](https://github.com/mtecnic/clusterspace) | Tiled workspace (Electron + React + TS) for terminals, embedded Chromium, and SSH panes **auto-wrapped in tmux** — sessions survive disconnects, restarts, and closing the app. Optional AI co-pilot gets **71 callable tools** (including ~50 browser-automation tools and a pair of vision tools that *judge what's on screen after an action*) and can drive any pane toward a goal. Tracks eight concurrent agents live. | ![★](https://img.shields.io/github/stars/mtecnic/clusterspace?style=flat-square&label=★) |
| [**code-atlas**](https://github.com/mtecnic/code-atlas) | Any repo as a navigable 3D world — city (height = LOC, glow = git churn), dependency galaxy, and molecule view of one file's symbols; the three **morph into each other**. Tarjan-SCC cycle detection, hotspot/ownership/coverage lenses, git time-scrub, local-LLM integration. Rendered vLLM — **~6,000 files, 1.5M LOC, 23,574 resolved imports** — in seconds. | ![★](https://img.shields.io/github/stars/mtecnic/code-atlas?style=flat-square&label=★) |
| [**ramp**](https://github.com/mtecnic/ramp) | *Real Agents Maximizing Productivity* — a local-first coding agent harness. One transport, one swappable wire per vendor: every local OpenAI-compatible server is just a URL, and Anthropic / Gemini / Bedrock are spoken in their own dialects behind the same seam. Most of what's in it is there because a specific run went wrong — the budget module exists because one afternoon produced **273 context overflows in 52 minutes**; `max_tokens` is derived from the window because **5,949 recorded tool calls** showed what responses actually need. **~4,000 hermetic tests**, random order. | ![★](https://img.shields.io/github/stars/mtecnic/ramp?style=flat-square&label=★) |
| [**cliide**](https://github.com/mtecnic/cliide) | The AI-native terminal IDE — file tree, editor, and a tool-executing agent in one TUI. Built AI-first rather than AI-bolted-on. On PyPI. | ![★](https://img.shields.io/github/stars/mtecnic/cliide?style=flat-square&label=★) |

---

## 📏 Measured, not vibed

Every figure from a repo you can clone, on hardware I own:

| Result | Where |
|---|---|
| **+136%** throughput and **~2×** lower latency from *less* tensor parallelism (2×TP=2 vs 1×TP=4, 4× RTX 3090, no NVLink) | [vllm-topology-bench](https://github.com/mtecnic/vllm-topology-bench) |
| **−4.9%** bits/char and **−4.2%** tokens on held-out code — HumanEval within noise, greedy generation byte-identical | [tokopt](https://github.com/mtecnic/tokopt) |
| **−18%** INT4 model size (4.4 → 3.6 GB) after tokenizer surgery + calibration, for ~25 min of one-time cost | [tokopt](https://github.com/mtecnic/tokopt) |
| **20%** of MoE experts removed (512 → 410/layer) from a 149 GB coder model, then W4A16 | [REAP-AWQ](https://github.com/mtecnic/research-test-Qwen3-Coder-Next-REAP-AWQ) |
| **−87%** bytes / **~90%** fewer tokens on real web pages, hierarchy and citations preserved | [ctx](https://github.com/mtecnic/ctx) |
| **146** apps built autonomously; **26** published unedited with per-check validation status | [cadillac](https://github.com/mtecnic/cadillac) · [builds](https://github.com/mtecnic/cadillac-builds) |
| **273** context overflows in 52 minutes — the failure that became a budget module | [ramp](https://github.com/mtecnic/ramp) |

## 🧾 House rules

- **Reproduce or it didn't happen.** Research repos ship a REPRODUCE.md you can actually follow, not a citation to a vibe.
- **Negative results get published.** tokopt's paper includes the method that *didn't* beat the baseline (hierarchical merge-tree init) right next to the ones that did. `4× TP=1` OOMing is a headline finding, not a footnote.
- **Validation status is public.** cadillac-builds lists every failed check, per project. Nothing is dressed up to look better than it is.
- **Runs on hardware you can own.** Consumer GPUs throughout. No cloud dependency, no API key required to reproduce anything above.
- **Point it at your own model.** Every tool here takes a URL. If it only works with someone else's API, it isn't finished.

---

<details>
<summary><b>🎲 Other things I've built</b> — not local-AI, still fun</summary>

<br>

| Repo | What it is | ★ |
|---|---|---|
| [**homestead**](https://github.com/mtecnic/homestead) | Sims-style house builder in the browser. Draw walls, rooms **detect themselves**, then paint, furnish, and walk around inside. three.js + TS, 4 runtime deps, 475 tests, zero art assets — everything generated at runtime. | ![★](https://img.shields.io/github/stars/mtecnic/homestead?style=flat-square&label=★) |
| [**neon**](https://github.com/mtecnic/neon) | Cyberpunk trading game where the currency is hardware. Build rigs from GPUs, RAM and CPUs; run an empire. | ![★](https://img.shields.io/github/stars/mtecnic/neon?style=flat-square&label=★) |
| [**matrix-doom**](https://github.com/mtecnic/matrix-doom) | Matrix-themed ASCII FPS raycaster in pygame — built autonomously by an LLM code generator, shipped unedited. | ![★](https://img.shields.io/github/stars/mtecnic/matrix-doom?style=flat-square&label=★) |
| [**airowling-novels**](https://github.com/mtecnic/airowling-novels) | **16 novels · 553,959 words · 221 chapters**, written end-to-end by an AI authoring pipeline. All first-pass, no human rewriting. | ![★](https://img.shields.io/github/stars/mtecnic/airowling-novels?style=flat-square&label=★) |
| [**cardboard**](https://github.com/mtecnic/cardboard) | Full-stack social platform for sports card collectors — share, buy, sell, trade. | ![★](https://img.shields.io/github/stars/mtecnic/cardboard?style=flat-square&label=★) |
| [**koding**](https://github.com/mtecnic/koding) | CodeQuest — gamified visual Python learning for kids 9+. | ![★](https://img.shields.io/github/stars/mtecnic/koding?style=flat-square&label=★) |
| [**cblchat**](https://github.com/mtecnic/cblchat) | Real-time enterprise chat with LDAP / Active Directory auth. | ![★](https://img.shields.io/github/stars/mtecnic/cblchat?style=flat-square&label=★) |

</details>

---

<div align="center">

**[wAIve.online](https://waive.online)** — self-hosted AI platform · **Fox Valley AI Foundation** — open tooling for everyone else

*Oshkosh, Wisconsin.*

</div>
