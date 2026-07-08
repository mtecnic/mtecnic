<div align="center">

```
  ███╗   ███╗████████╗███████╗ ██████╗███╗   ██╗██╗ ██████╗
  ████╗ ████║╚══██╔══╝██╔════╝██╔════╝████╗  ██║██║██╔════╝
  ██╔████╔██║   ██║   █████╗  ██║     ██╔██╗ ██║██║██║
  ██║╚██╔╝██║   ██║   ██╔══╝  ██║     ██║╚██╗██║██║██║
  ██║ ╚═╝ ██║   ██║   ███████╗╚██████╗██║ ╚████║██║╚██████╗
  ╚═╝     ╚═╝   ╚═╝   ╚══════╝ ╚═════╝╚═╝  ╚═══╝╚═╝ ╚═════╝
```

### I run a card shop. The GPU cluster is load-bearing.

Self-hosted AI out of Oshkosh, WI — **[wAIve.online](https://waive.online)** · Fox Valley AI Foundation

![Python](https://img.shields.io/badge/python-3.12+-3776AB?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/typescript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)
![vLLM](https://img.shields.io/badge/serving-vLLM-orange?style=flat-square)
![Hardware](https://img.shields.io/badge/hardware-RTX%203090%20cluster-76B900?style=flat-square&logo=nvidia&logoColor=white)
![Cloud](https://img.shields.io/badge/cloud-none-2ea44f?style=flat-square)

</div>

---

Everything here runs on hardware I can physically kick. Two threads tie it together: **agents that ship finished, validated software** — not snippets — and **making big models cheaper to run** through tokenizer surgery, expert pruning, and token-efficient context formats. Results get published either way, including the negative ones.

---

## 🔬 Model surgery & token economics

Smaller models, fewer tokens, same behavior — with the measurements to prove it.

| Repo | What it does | ★ |
|---|---|---|
| [**tokopt**](https://github.com/mtecnic/tokopt) | Post-hoc BPE tokenizer adaptation for deployed LLMs — script-aware pruning, continued BPE extension, embedding-only calibration. Validated end-to-end on Qwen3.5-4B; reproduces in ~3 hrs on a single RTX 3090. Ships a ~9,000-word PAPER.md and a step-by-step REPRODUCE.md. | ![★](https://img.shields.io/github/stars/mtecnic/tokopt?style=flat-square&label=★) |
| [**research-test-Qwen3-Coder-Next-REAP-AWQ**](https://github.com/mtecnic/research-test-Qwen3-Coder-Next-REAP-AWQ) | REAP expert pruning + AWQ quantization of Qwen3-Coder-Next (149 GB BF16 MoE). 512 → 410 experts per layer via 4-dataset saliency calibration with super-expert preservation, then W4A16 @ group_size 32 for consumer GPUs. | ![★](https://img.shields.io/github/stars/mtecnic/research-test-Qwen3-Coder-Next-REAP-AWQ?style=flat-square&label=★) |
| [**ctx**](https://github.com/mtecnic/ctx) | CTX (Context Transfer Format) — an interchange format for LLM web consumption. Turns a 1.2 MB Wikipedia page into 150 KB of structure-preserving CTX: −87% bytes, ~90% fewer tokens. CLI, Python lib, and a FastAPI service with Redis caching + transparent proxy. | ![★](https://img.shields.io/github/stars/mtecnic/ctx?style=flat-square&label=★) |

## 🤖 Agents that ship

Autonomy is easy. *Finished* is the hard part.

| Repo | What it does | ★ |
|---|---|---|
| [**cadillac**](https://github.com/mtecnic/cadillac) | Autonomous coding agent: a sentence in, a validated app out. 12-stage pipeline (SPEC → … → CRITIC → RUNTIME → PACKAGE) with operational gates, a completeness CRITIC, runtime flow verification, and surgical-mode stuck-loop recovery. 616 tests passing · 146 apps built · 403 lessons learned. | ![★](https://img.shields.io/github/stars/mtecnic/cadillac?style=flat-square&label=★) |
| [**cadillac-builds**](https://github.com/mtecnic/cadillac-builds) | The receipts: 26 runnable applications built unattended by Cadillac with zero human edits — published from a scan of ~99 build workspaces, failures acknowledged. Every project's README lists exactly which validation checks passed and which didn't. | ![★](https://img.shields.io/github/stars/mtecnic/cadillac-builds?style=flat-square&label=★) |
| [**workerAI**](https://github.com/mtecnic/workerAI) | Agentic workflow system on vLLM + LangGraph — a ReAct-style autonomous agent that plans, reasons, and executes multi-step tasks with a library of specialized tools. | ![★](https://img.shields.io/github/stars/mtecnic/workerAI?style=flat-square&label=★) |

**Things the agents made** (research artifacts, shipped as-is):

| Artifact | What it is | ★ |
|---|---|---|
| [**matrix-doom**](https://github.com/mtecnic/matrix-doom) | Matrix-themed ASCII FPS raycaster in pygame — built autonomously by a modular LLM code generator. | ![★](https://img.shields.io/github/stars/mtecnic/matrix-doom?style=flat-square&label=★) |
| [**silence-of-lolth**](https://github.com/mtecnic/silence-of-lolth) | A 33,152-word, 14-chapter novel written end-to-end by an AI-authoring pipeline (AIRowling), voice-referenced on R.A. Salvatore's Dark Elf Trilogy. | ![★](https://img.shields.io/github/stars/mtecnic/silence-of-lolth?style=flat-square&label=★) |

## 🖥️ The cockpit

Tools for running a multi-node cluster from one chair.

| Repo | What it does | ★ |
|---|---|---|
| [**clusterspace**](https://github.com/mtecnic/clusterspace) | Tiled desktop workspace (Electron + React + TS) for terminals, embedded Chromium, and SSH auto-wrapped in tmux — sessions survive everything. Optional AI co-pilot gets first-class tools to read, type into, and drive any pane toward a goal. | ![★](https://img.shields.io/github/stars/mtecnic/clusterspace?style=flat-square&label=★) |
| [**model-chat-cli**](https://github.com/mtecnic/model-chat-cli) | Terminal command center for local AI servers. Auto-discovers Ollama, LM Studio, and vLLM on your network; chat with real TTFT/decode metrics, blind multi-model battles with auto-judged tournaments, load tests, and a 45-task agentic benchmark across 6 difficulty tiers. | ![★](https://img.shields.io/github/stars/mtecnic/model-chat-cli?style=flat-square&label=★) |
| [**cliide**](https://github.com/mtecnic/cliide) | The AI-native terminal IDE — file tree, editor, and an agent with tool execution in one TUI. Built AI-first rather than AI-bolted-on. On PyPI. | ![★](https://img.shields.io/github/stars/mtecnic/cliide?style=flat-square&label=★) |

## 🧰 Shop floor & one-offs

| Repo | What it does | ★ |
|---|---|---|
| [**cardboard**](https://github.com/mtecnic/cardboard) | Full-stack social platform for sports card collectors — share, buy, sell, trade. The day job and the code base, converging. | ![★](https://img.shields.io/github/stars/mtecnic/cardboard?style=flat-square&label=★) |
| [**cblchat**](https://github.com/mtecnic/cblchat) | Real-time enterprise chat with LDAP / Active Directory auth. | ![★](https://img.shields.io/github/stars/mtecnic/cblchat?style=flat-square&label=★) |
| [**koding**](https://github.com/mtecnic/koding) | CodeQuest — gamified, visual Python learning platform for kids 9+. | ![★](https://img.shields.io/github/stars/mtecnic/koding?style=flat-square&label=★) |
| [**Cbl**](https://github.com/mtecnic/Cbl) | Native iOS WebView wrapper for a WordPress storefront (Swift). | ![★](https://img.shields.io/github/stars/mtecnic/Cbl?style=flat-square&label=★) |

---

## 📏 Measured, not vibed

Numbers pulled from the repos, on my own hardware:

| Result | Where |
|---|---|
| **−4.9%** bits/char and **−4.2%** tokens on held-out code, HumanEval within noise, greedy generation byte-identical | [tokopt](https://github.com/mtecnic/tokopt) |
| **−18%** INT4 model size (4.4 GB → 3.6 GB) after tokenizer surgery + calibration | [tokopt](https://github.com/mtecnic/tokopt) |
| **20%** of MoE experts removed (512 → 410/layer) from a 149 GB coder model, then W4A16 | [REAP-AWQ](https://github.com/mtecnic/research-test-Qwen3-Coder-Next-REAP-AWQ) |
| **−87%** bytes / **~90%** fewer tokens on real web pages | [ctx](https://github.com/mtecnic/ctx) |
| **146** applications built autonomously; **26** published unedited with per-check validation status | [cadillac](https://github.com/mtecnic/cadillac) · [builds](https://github.com/mtecnic/cadillac-builds) |

## 🧾 House rules

- **Reproduce or it didn't happen.** Research repos ship a REPRODUCE.md you can actually follow, not a citation to a vibe.
- **Negative results get published.** tokopt's paper includes the method that *didn't* beat the baseline (hierarchical merge-tree init) right next to the ones that did.
- **Validation status is public.** cadillac-builds lists every failed check per project. No project is dressed up to look better than it is.
- **Runs on hardware you can own.** Everything above was built and validated on consumer GPUs — no cloud dependency, no API key required to reproduce.

---

<div align="center">

**[wAIve.online](https://waive.online)** — self-hosted AI platform · **Fox Valley AI Foundation** — open tooling for everyone else

*Oshkosh, Wisconsin. Yes, really.*

</div>
