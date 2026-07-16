# Mac + MLX Megathread — Hermes Agent on Apple Silicon (June 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1uc7rw5/mac_mlx_megathread_hermes_agent_on_apple_silicon/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

LAST UPDATED: June 21, 2026

This is the every-question-answered reference for running Hermes Agent locally on Apple Silicon. Aggregated from 20+ r/hermesagent threads, GitHub issue trackers, independent benchmarks, and model documentation — all sourced within the last 45 days.

---

## TL;DR — What Should I Download Right Now?

| Your Mac | Download This | Backend | Why |
|----------|--------------|---------|-----|
| 8GB | Qwen3.5-4B Q4_K_M or Gemma 4 E2B Q4 | Ollama or llama.cpp | Accept the limits. Use for simple chat, not heavy agent work. |
| 16GB | Qwen3.5-9B Q4_K_M or MLX 4-bit | llama.cpp (best compatibility) or Ollama | The practical floor for Hermes. Preserve RAM for context — use quantized KV cache. |
| 24GB | Qwen3.6-27B Q4_K_M OR Qwen3.6-35B-A3B 4-bit MLX | llama.cpp for dense; MLX-LM for MoE | Dense = stronger coding/predictability. MoE = much faster decode (~60 tok/s on Max). |
| 32-48GB | Qwen3.6-35B-A3B 4-bit MLX (OptiQ if available) | MLX-LM or oMLX | The current Mac sweet spot. ~3B active per token, ~20GB file, runs like a 3B model. |
| 48-64GB | Qwen3.6-27B Q6_K / Q8, or Qwen3.6-35B-A3B 8-bit | llama.cpp for dense quality; MLX for MoE speed | Dense Q6/Q8 is the serious-agent quant. |
| 64-96GB | Gemma 4 26B-A4B Q4 as MoE alternative, Qwen3.6-27B Q8 | MLX or llama.cpp | Alternative MoE if you want non-Qwen. |
| 128GB+ | Same as 64GB+ at better quants. DeepSeek V4 Flash experimental. | Same as above | More RAM = better quants + longer context. Not automatically a better model. |

**Stock models first.** Uncensored variants (Heretic, HauhauCS) are advanced options — test tool calling before relying on them. The boring model that follows schema for 6+ tool calls beats the spicy one that talks itself into a ditch.

---

## The Backend Landscape — Which Server to Use

### llama.cpp (GGUF)
**Best for:** maximum compatibility, KV cache control, vision/mmproj, Jinja templates.

```bash
llama-server -m ~/models/model.gguf \
  -ngl 99 -c 65536 -np 1 -fa on \
  --cache-type-k q8_0 --cache-type-v q4_0 \
  --host 127.0.0.1 --port 8080 --jinja
```

- Fastest time-to-first-token (TTFT) in Hermes' own testing
- Full KV cache quantization control (critical for 16GB Macs)
- MTP is a NET LOSS on Metal — do not enable it. Baseline Qwen3.5-9B at 25.3 tok/s → MTP drops to 19.3 tok/s at best, 1.93 tok/s on Qwen3.6-35B self-MTP (13.6x slower). See llama.cpp issues #23752 and #23011.
- For 16GB: start at 64K context, not 128K. Move up only if needed.

### MLX-LM / oMLX
**Best for:** maximum generation speed on MoE models.

```bash
pip install mlx-lm
mlx_lm.server --model mlx-community/Qwen3.6-35B-A3B-4bit --port 8000
```

- 20-30% faster than llama.cpp on generation, gap widens on larger models
- Qwen3.6-35B-A3B 4-bit: 61.2 tok/s vs 16.7 tok/s for dense 27B 4-bit (M1 Max 64GB test)
- KNOWN BUGS (June 2026):
  - Qwen3.5/3.6 non-Coder models may fail to emit tool_calls — parser auto-detection doesn't match their chat templates (mlx-lm issue #1293)
  - MTP variants return 1-2 token completions on second requests (mlx-lm issue #1292) — use non-MTP MLX variants for multi-turn
  - Prompt cache failure on Qwen3-Next hybrid architectures (mlx-lm issue #1162)

### Ollama
**Best for:** easiest setup, zero config.

```bash
brew install ollama
ollama pull qwen3.6:35b-a3b-mlx
ollama serve
```

- MLX backend since v0.19: 2x decode speed, ~1.6x prefill speed on M5 Max
- v0.30.x added Gemma 4 MLX support and flash attention auto-enable
- Community split: some users prefer Ollama MLX for snappiness and RAM distribution; others prefer raw MLX-LM server for speed (Ollama is still a llama.cpp wrapper underneath for non-MLX models)
- KNOWN BUGS (June 2026):
  - KV cache memory leak: v0.30.8 on M4 Max 64GB, memory grows from ~24GB to 75GB, token generation collapses under swap (issue #16698)
  - Inter-prompt delay regression on Apple Silicon MLX (issue #16170, v0.24.0)
  - If Hermes loops tools or gets slow, test the same model through llama.cpp or MLX-LM before blaming the model

### LM Studio
**Best for:** GUI model management, non-terminal users.

- Exposes OpenAI-compatible API on port 1234
- Supports both GGUF and MLX paths
- Community experience: even on 36GB M4 Max with Gemma 26B, one user reports "it randomly forgets about installed tools, fails to report expired tokens, sometimes doesn't answer at all"
- KNOWN BUGS:
  - Tool-calling parser breaks Qwen3.5 in some versions — generates correct JSON but LM Studio truncates or misparses it
  - Gemma 4 tool calls: MLX backend leaks raw function markers into message content; GGUF backend has a working parser but runs ~50% slower
  - Upgrade to latest version before troubleshooting

### Rapid-MLX (NEW — June 2026)
**Best for:** maximum speed on Apple Silicon, especially agentic/tool-calling workloads.

- 2-4x faster than Ollama, 0.08s cached TTFT
- 17 tool parsers, 100% tool calling support
- Drop-in OpenAI replacement — works with Hermes, Claude Code, Cursor, Aider
- 3,000+ GitHub stars, actively maintained (commits as of June 21, 2026)
- Currently the strongest Mac backend for tool-calling reliability

### Lightning MLX
Fork of Rapid-MLX optimized for agentic use — short streamed turns, tool calls, low-latency interactions. Claims 220 tok/s on compatible hardware.

---

## Model Deep Dive

### Qwen3.6-35B-A3B — The Current Mac Default (32GB+)

Released April 16, 2026. MoE: 35B total, ~3B active per token. Apache 2.0.

- GPU QA Diamond: 86.0. SWE-bench: 73.4%.
- Native context: 262K (extendable to ~1M via YaRN)
- Best quants: MLX 4-bit (~20.4GB), OptiQ 4-bit (mixed 4/8-bit, better BFCL/HashHop)
- Uncensored: HauhauCS Aggressive, DavidAU Heretic (advanced only)
- Chat template: use Jinja. Tool calls require correct template — stock is fine on modern runtimes, fall back to `spiritbuun/buun-Qwen3.6-chat_template` if breaking

**Sampling for Hermes agent work:**
- Thinking ON, temp 0.6, top_p 0.95, top_k 20 (coding/tools)
- Thinking ON, temp 0.8-1.0, top_p 0.95, top_k 20 (research/chat)
- Non-thinking: temp 0.7, top_p 0.8, top_k 20, presence_penalty 1.5

### Qwen3.6-27B Dense — Best Dense Coding Pick (24GB+)

Released April 22, 2026. 27B dense. Apache 2.0. SWE-bench Verified: 77.2.

- Simon Willison clocked Unsloth Q4_K_M at 25.57 tok/s with flagship-class results
- Best quants: Q4_K_M (24GB, 16.8GB file), Q5_K_M (32GB+), Q6_K/Q8 (48GB+)
- Q6 is the preferred serious-agent quant — Q4 works but can drift on long tool traces
- Heretic NEO-CODE variant: 523K+ downloads (DavidAU, last modified June 11, 2026)

### Qwen3.5-9B — Best 16GB Floor

Mature, well-documented, official Hermes Mac starter model.

- Best quants: Q4_K_M GGUF (practical default) or MLX 4-bit
- Q6_K if you have room — better tool reliability
- Official Hermes docs recommend it as the starter model for constrained Macs
- HauhauCS Aggressive available for uncensored use

### Gemma 4 Family — Best Cross-Family Alternative

- **Gemma 4 12B:** Google positions for local agentic/multimodal laptop workflows (June 3, 2026 post). Viable 16-24GB candidate.
- **Gemma 4 26B-A4B:** 26B total, ~4B active. Good 32GB+ MoE alternative. 256K context.
- **Gemma 4 31B:** Dense. 64GB+ preferred.
- **Gemma 4 E2B/E4B:** Lighter models. Good as fast aux/routing models.
- Known issue: mac-llm-bench scores unusually low on Gemma 4 due to llama.cpp tool-calling bug causing premature inference stopping — not actual model capability.
- Thinking mode: add `<|think|>` at start of system prompt. Don't feed prior thought blocks back into history.

---

## Critical Pitfalls — Read This Before You Waste 4 Hours

### 1. MTP = Slower on Mac (Not Faster)
Unsloth and model cards market 1.4-2.2x speedups from MTP. On Apple Silicon Metal, it's a NET LOSS at every configuration. llama.cpp #23752 confirms: baseline 25.3 tok/s → MTP 19.3 tok/s (-24%). Qwen3.6-35B self-MTP: baseline 26.2 tok/s → MTP 1.93 tok/s (-92%). Do not enable `--spec-type draft-mtp` on Mac.

### 2. Tool Calling Breaks Across Backends
The tool-calling stack on Mac is the #1 failure mode for Hermes users:
- **MLX-LM:** Qwen3.5/3.6 non-Coder tool parser mismatch
- **Ollama MLX:** KV cache leak causes swap death, tool loops hang
- **LM Studio:** Parser truncates Qwen tool calls, Gemma MLX leaks raw markers
- **Fix:** test `/v1/chat/completions` with a tool-calling prompt before trusting any backend. If one backend fails, try another before blaming the model.

### 3. 16GB Is the Floor, Not the Sweet Spot
Usable memory after macOS and apps is ~10-12GB. That's Qwen3.5-9B Q4_K_M + 64K context with quantized KV. Forcing a 14B/27B into swap will feel terrible. Use a strong 9B at a decent quant over a crippled larger model.

**And 2B models are not viable for Hermes tool use.** One user tested gemma4:e2b on a Mac Mini M4 16GB with oMLX: "can't even handle one request — 'list my Google Drive files'." Gemma4:e4b generated invalid tool calls. Qwen3.5-9B "spent 15 minutes... sort of worked." Community consensus: 7B-9B is the minimum for tool calling. 2B-4B models are chatbots, not agents.

### 4. Bandwidth > Chip Generation
An M3 Max (400 GB/s) generates tokens faster than an M4 Pro (273 GB/s). For LLM inference, memory bandwidth is the bottleneck. When shopping Macs for Hermes: Max > Pro > base, even if it's a generation older.

### 5. Agent Context Tax — Hermes Burns Tokens Just to Say "Hi"
Multiple users report that Hermes' orchestrator overhead consumes massive context before the model even starts working. One user measured: "when I say 'hi' the agent takes around 15K tokens to reply." Another: "you could be waiting for a reply and it's off carefully writing a new skill. I've walked away, thinking it had died and 30 minutes later it returns."

This is especially punishing on local Macs where context = RAM. Fixes:
- Use quantized KV cache to reduce the memory cost of bloated context
- Keep context windows conservative (64K, not 128K+) unless your workflow demands it
- Consider using a smaller/faster model as orchestrator and delegating heavy work to cloud sub-agents
- One user fine-tunes Gemma 4 E4B as a lightweight orchestrator that delegates to frontier models

### 6. KV Cache Is the Hidden Memory Killer
Context eats RAM faster than model weights. On a 16GB Mac with Qwen3.5-9B Q4_K_M, 128K context with fp16 KV cache can use 8GB for cache alone. Always use quantized KV:
```bash
--cache-type-k q8_0 --cache-type-v q4_0
```
Ollama users: set `OLLAMA_KV_CACHE_TYPE=q8_0` and `OLLAMA_FLASH_ATTENTION=1`.

**Proven fix from the community:** One M3 Pro 18GB user went from timeouts to "now I can have a conversation" by combining KV cache 4-bit quantization + Hermes 0.8.0 lazy skill loading + 40K context window. First message still ~14K tokens but subsequent drops to ~600 tokens.

### 7. Qwen Overthinks — Turn Thinking Off on Constrained Macs
Qwen models are known to overthink on even the simplest prompts. On RAM-limited Macs, long thinking chains plus Hermes' already-heavy system prompt can push you into timeout territory instantly. A simple fix that worked for multiple users: turn thinking off. It's not ideal for complex multi-step tasks, but it's the difference between "works" and "stares at a wall for 15 minutes then times out" on 16-18GB machines.

---

## Hermes Agent Setup Commands

For any local OpenAI-compatible server:

```bash
# Verify your model endpoint works
curl -s http://127.0.0.1:8080/v1/models

# Configure Hermes
hermes config set model.provider custom:local-mac
hermes config set model.base_url http://127.0.0.1:8080/v1
hermes config set model.api_key local-no-key
hermes config set model.default MODEL_ID_FROM_ABOVE
```

For the Desktop app (released June 2, 2026, v0.15.2): same config pattern, set the provider to your local endpoint.

---

## What the Community Is Actually Running

These are real setups from r/hermesagent users in May-June 2026:

| User | Hardware | Model | Speed | Notes |
|------|----------|-------|-------|-------|
| Britbong1492 | M4 Max | Qwen3.6:35B-A3B local | ~80 t/s, TTFT 0.3s | 95% local, 5% Kimi k2.6 fallback. ~$1/week/agent |
| 310dweller | M4 Mac mini 32GB | Gemma 4 26B 4-bit MLX via oMLX | faster than Ollama cloud | "Thinking loops locally" were tricky to tune |
| Tacamaniac | M1 Max 64GB | Qwen 7B + 35B on MLX | — | Migrated from Docker to native macOS. Testing vs GPT-5.5: "It performs better. However in 10 minutes of trying, it cost me $1.50." |
| italianamerican985 | M1 Ultra 128GB | Qwen3.6-35B Q4 | 80 t/s | "Felt like it made a lot of mistakes compared to Minimax or 27B." MTP did not produce speed gains |
| EfficientShop5015 | M2 Max 32GB | — | — | "Usable locally but nothing like API." |
| TanguayX | M2 Ultra 64GB | Qwen3.5, Gemma 4 | — | "Context window second half is useless as it's so slow." Notes Hermes orchestrator overhead as bottleneck, not model speed |
| asankhs | Apple Silicon | Qwen3.5-9B OptiQ 4-bit | — | Recommends OptiQ quants for Mac: huggingface.co/mlx-community/Qwen3.5-9B-OptiQ-4bit |
| BassAzayda | 16GB VRAM GPU | Qwen3.6 35B A3B | — | "Done over 42 tool calls no problems — first time I had a stable conversation" |
| GyGeek | Framework Desktop 128GB | Qwen3.6-27B-Q8_0 | — | "Kind of slow but effective." AMD Ryzen AI Max+ 395, Radeon 8060S |

**Takeaway:** The M4 Max + Qwen3.6:35B-A3B is emerging as the community's best local Hermes setup. M1/M2 Ultra users with more RAM are hitting different bottlenecks — agent overhead, context window slowdown, not model capability.

---

## FAQ — Real Questions from r/hermesagent

**Q: I have a Mac Mini M4 32GB. Why is Qwen3.5-9B failing at tool calling?**
A: It's likely not the model — it's the backend. If you're on Ollama, try the MLX backend explicitly (`qwen3.5:9b-mlx`). If that fails, switch to llama.cpp or test Rapid-MLX. Qwen3.5-9B is capable of tool calling; backends often break the parser.

**Q: Can I run Hermes on a 16GB MacBook?**
A: Yes — use Qwen3.5-9B Q4_K_M with quantized KV cache and 64K context. Don't expect heavy multi-tool sessions. It works for research, chat, and simple automation.

**Q: Why is my model generating 3 tok/s when benchmarks say 25+?**
A: Three common causes: (1) model fell back to CPU — verify with `-ngl 99` in llama.cpp or check Ollama GPU usage; (2) you enabled MTP — disable it immediately on Mac; (3) your context window is too large and you're in swap.

**Q: MLX or GGUF — which is better for Hermes?**
A: MLX for generation speed, especially on MoE models. GGUF/llama.cpp for KV cache control, vision support, and maximum compatibility. For Hermes tool loops, llama.cpp's predictable behavior often wins despite slower raw generation.

**Q: Should I use Ollama, LM Studio, or raw llama.cpp/MLX-LM?**
A: Ollama for easiest setup. LM Studio for GUI comfort. llama.cpp/MLX-LM for maximum control and debugging. Rapid-MLX if speed and tool-calling reliability are your top priorities.

**Q: Is Qwen3.6-35B-A3B good enough to replace cloud models?**
A: For most Hermes workflows — yes. 35B parameters with MoE delivers flagship-class behavior on Mac. The community consensus is that it's the 2026 default for 32GB+ Macs. It won't match GPT-5.5/Claude on extremely complex multi-step reasoning, but it handles files, code, web, and structured output reliably.

**Q: What about uncensored models for Hermes?**
A: Start with stock. If you want uncensored, Heretic and HauhauCS Balanced are the best-documented. Aggressive/Huihui builds exist but test tool calling before relying on them. Uncensored ≠ better agent.

**Q: I have an M1 Ultra 128GB and I'm struggling. Why?**
A: This is a real question from the subreddit. More RAM doesn't solve backend bugs. Verify your backend (Ollama vs llama.cpp), check for MTP being enabled, test a simple model first, and make sure KV cache is quantized. But also: one user found that "the agent itself eats up a lot of memory/resources — when I say 'hi' the agent takes around 15K tokens to reply." The hardware is capable — the software stack and agent overhead are usually the bottlenecks.

**Q: Does the M5 chip make a big difference for Hermes?**
A: M5 Max has ~614 GB/s bandwidth vs M4 Max's ~546 GB/s — about 12% faster token generation. The bigger story is the M5 Neural Accelerators, which Apple claims give 4x TTFT gains for MLX models. Real-world: modest improvement, but not game-changing for LLM inference vs. M4 Max. Supply is constrained as of June 2026.

**Q: Gemma 4 or Qwen3.6 — which should I use?**
A: Qwen3.6-35B-A3B for speed and agentic coding. Gemma 4 26B-A4B if you want a non-Qwen MoE alternative or need multimodal/image capabilities. Both are strong; Qwen has more community validation for Hermes specifically.

**Q: What's the fastest inference engine for Mac right now?**
A: Rapid-MLX (June 2026) claims 2-4x faster than Ollama with 100% tool calling. Lightning MLX claims 220 tok/s. Both are newer/less battle-tested than llama.cpp and Ollama. For reliability, stick with llama.cpp or Ollama. For speed, test Rapid-MLX.

**Q: Is the hybrid cloud/local approach worth it?**
A: Yes — it's the current community consensus. The top-voted comment on the biggest local-models thread (+35) recommends a $20 Codex plan as fallback. One M4 Max user runs 95% Qwen3.6:35B-A3B local (~80 t/s, TTFT 0.3s) and falls back to Kimi k2.6 at under $1 per million tokens, totaling about $1/week. This pattern — fast local for routine work, cheap cloud for hard tasks — is the practical sweet spot.

**Q: Should I use Hermes for coding on a local Mac?**
A: The community is split. Some (including the "Running Locally, Really?" OP) say "Hermes is not for coding — it's an orchestration agent. Use Claude Code or OpenCode as a sub-agent." Others successfully code with Qwen3.6-35B-A3B or Qwen3.6-27B locally. The consensus: Hermes excels at orchestrating — delegating coding tasks to specialized sub-agents. If you want a single model that directly writes code, the cloud frontier models still have an edge.

**Q: I have a brand-new M4 Max with 36-48GB. Will everything just work?**
A: Not automatically. One M4 Max 36GB user reports that even with Gemma 26B, Hermes "randomly forgets about installed tools, fails to report expired tokens, sometimes doesn't answer at all." The hardware is capable but the software stack — backend bugs, agent context overhead, tool calling edge cases — is the real bottleneck. Start with Qwen3.6-35B-A3B on MLX-LM or Ollama MLX, verify tool calling, and lower your context window before raising it.

---

## Quick Backend Decision Matrix

| You Want... | Use This | Avoid |
|-------------|----------|-------|
| Fastest setup | Ollama | — |
| Best tool-calling reliability | llama.cpp or Rapid-MLX | Ollama MLX (KV cache leak), LM Studio (parser bugs) |
| Fastest generation | Rapid-MLX or MLX-LM | — |
| Vision/multimodal | llama.cpp + mmproj | MLX-LM (text-only) |
| Long context (128K+) | llama.cpp + quantized KV + TurboQuant | Ollama on 16GB (memory leak risk) |
| GUI without terminal | LM Studio | — |
| Uncensored models | llama.cpp + Heretic/HauhauCS GGUF | MLX (fewer uncensored builds) |
| MTP speed boost | NVIDIA GPU, not Mac | MTP on Apple Metal (net loss) |

---

## Temperature & Sampling Quick Reference

For Hermes agent work with Qwen3.6:

| Workload | Thinking | Temp | Top-P | Top-K | Presence Penalty |
|----------|----------|------|-------|-------|-----------------|
| Coding / tool loops | ON | 0.6 | 0.95 | 20 | 0.0 |
| Research / chat | ON | 0.8-1.0 | 0.95 | 20 | 0.0 |
| Summarization (latency matters) | OFF | 0.7 | 0.8 | 20 | 1.5 |
| Precise structured output | ON | 0.6 | 0.95 | 20 | 0.0 |

For non-thinking mode: `--chat-template-kwargs '{"enable_thinking":false}'` in llama.cpp.

---

## Sources

All sources from the last 45 days (May 7 – June 21, 2026):

### Reddit Threads (r/hermesagent)
- Best Small Models for Hermes on Mac Mini M4: https://redd.it/1sq5802
- Running Hermes with Local Models: https://redd.it/1tegogu
- How to Run Hermes for Free: https://redd.it/1tqzwl9
- M1 Ultra 128GB Struggling: https://redd.it/1toi897
- Running Locally, Really?: https://redd.it/1snju8w
- I Curated the Best Local Models: https://redd.it/1stiwug
- Hermes Agent Using Local LLM: https://redd.it/1tvrb2r
- 4 Hours Setting Up Hermes Locally — Reality Check: https://redd.it/1u1u4ke
- Hermes and Local Qwen3.5-9B: https://redd.it/1sgogsr
- Yes, Hermes and Qwen3.5:4B Is All I Need: https://redd.it/1snfnq9
- Any Luck with Local Gemma4:E2B?: https://redd.it/1swl63u
- Best Free Model for Hermes: https://redd.it/1tijj0c
- Best Ollama Model for Local Only: https://redd.it/1u56mbi
- What Model Are You Running?: https://redd.it/1t3lscj
- AMA Summary from r/LocalLLaMA: https://redd.it/1szctp1
- Models Megathread May 2026: https://redd.it/1tgbsuz
- Looking for Hermes Best Practices: https://redd.it/1tlnmw3
- Qwen3.6-35B-A3B Definitive Guide: https://redd.it/1tmp2qy
- Qwen3.6-27B Definitive Guide: https://redd.it/1tn4lye
- Please Critique My Hermes Setup: https://redd.it/1ti2s9u

### GitHub Issues
- llama.cpp #23752: MTP degrades throughput on Metal (May 27, 2026)
- llama.cpp #23011: Qwen3.6-35B self-MTP much slower on Metal (May 13, 2026)
- mlx-lm #1293: Qwen3.5/3.6 tool parser mismatch
- mlx-lm #1292: Qwen3.6 MTP variant multi-turn failures
- mlx-lm #1162: Prompt-cache failure on Qwen3-Next hybrid
- Ollama #16698: MLX KV cache memory leak on M4 Max (v0.30.8)
- Ollama #16170: Inter-prompt delay regression on MLX (v0.24.0)

### Articles & Benchmarks
- Hermes Agent Docs — Run Local LLMs on Mac: https://hermes-agent.nousresearch.com/docs/guides/local-llm-on-mac
- InsiderLLM — Best Local LLMs for Mac 2026: https://insiderllm.com/guides/best-local-llms-mac-2026/
- Rapid-MLX: https://github.com/raullenchai/Rapid-MLX
- Lightning MLX: https://github.com/samuelfaj/lightning-mlx
- Apple ML Research — MLX on M5: https://machinelearning.apple.com/research/exploring-llms-mlx-m5
- ModelFit — Speculative Decoding on Mac: https://modelfit.io/blog/speculative-decoding-mac-llm/
- Unsloth — Qwen3.6 Local Guide: https://unsloth.ai/docs/models/qwen3.6
- Unsloth — Gemma 4 Local Guide: https://unsloth.ai/docs/models/gemma-4
- HuggingFace — mlx-community/Qwen3.6-35B-A3B-4bit: 93K downloads/month
- HuggingFace — mlx-community/Qwen3.6-35B-A3B-OptiQ-4bit: modified June 19, 2026
- HuggingFace — Ornstein-Hermes-3.6-27B-MLX-6bit: ~22.6GB, good for 32GB Macs
- mac-llm-bench: https://github.com/enescingoz/mac-llm-bench
- LM Studio Changelog: https://lmstudio.ai/changelog
- Ollama MLX Performance: https://craftrigs.com/news/ollama-0-19-mlx-apple-silicon-speed/
- Local AI Master — Mac Buying Guide: https://localaimaster.com/blog/apple-silicon-ai-buying-guide
- LLMCheck Benchmarks: https://llmcheck.net/benchmarks
- SitePoint — Local LLMs on Apple Silicon 2026: https://www.sitepoint.com/local-llms-apple-silicon-mac-2026/
- Ollama MLX vs Metal: https://andrew.ooo/answers/ollama-mlx-vs-ollama-metal-apple-silicon-2026/
- LM Studio Apple Silicon Errors & Fixes: https://ianlpaterson.com/blog/lm-studio-fix-cannot-truncate-prompt-n-keep-n-ctx/
- Google AI — Gemma 4 12B Local Agentic: https://developers.googleblog.com/bringing-gemma-4-12b-to-your-laptop-unlocking-local-agentic-workflows-with-google-ai-edge/

---

## Contribute

Found a better config? Discovered a bug fix? Running a model/backend combo not listed here? Drop it in the comments. This megathread updates as the stack evolves.

*Last refreshed: June 21, 2026. Models, backends, and bugs change weekly — check post dates on linked sources.*

---

## Comment-Sourced Updates

- **Shared-machine contention:** running the desktop/UI and local inference on the same constrained Mac can compete for memory/GPU resources. A TUI or split host/VM architecture may be more stable.

- **Backend benchmarks vary:** Rapid-MLX, oMLX MTP, LM Studio, llama.cpp, and other throughput reports depend on model, quant, context, cache, OS, and hardware. Treat reported tokens/sec as examples, not rankings.

- **Unresolved candidates:** Apple Container support, Atomic.Chat, DwarfStar, NEX-N2 mini, and other comment suggestions require current compatibility checks before recommendation.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
