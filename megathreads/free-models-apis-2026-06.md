# Free Models and APIs for Hermes Agent — Megathread (June 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1uj9nkn/free_models_apis_for_hermes_agent_megathread_june/
>
> This file preserves the original megathread content and adds comment-sourced updates where users clarified practical usage, GitHub maintenance, and contribution workflow.


**LAST UPDATED: June 29, 2026**\
**Scope:** Free models available via OpenRouter and other free API
sources — what's available, what the community uses, and what works for
different use cases.\
**Sources:** [r/hermesagent](https://www.reddit.com/r/hermesagent) community threads,
OpenRouter model listings/category rankings, API provider documentation.

------------------------------------------------------------------------

## Part 1: TL;DR — Quick Picks

| Decision | Community Pick | Runner-Up |
|----|----|----|
| Best free general-purpose | [DeepSeek V4 Flash](https://openrouter.ai/deepseek/deepseek-v4-flash:free) | [Owl Alpha](https://openrouter.ai/openrouter/owl-alpha) |
| Best free coding | [Laguna M.1](https://openrouter.ai/poolside/laguna-m.1:free) (Programming \#12) | [Qwen3 Coder 480B](https://openrouter.ai/qwen/qwen3-coder:free) |
| Best free reasoning/orchestration | [Nemotron 3 Ultra](https://openrouter.ai/nvidia/nemotron-3-ultra-550b-a55b:free) (55B active) | [gpt-oss-120b](https://openrouter.ai/openai/gpt-oss-120b:free) |
| Best free for specific prompting | [Owl Alpha](https://openrouter.ai/openrouter/owl-alpha) | DeepSeek V4 Pro |
| Best free multimodal | [Gemma 4 31B](https://openrouter.ai/google/gemma-4-31b-it:free) | [Nemotron Nano 12B V2 VL](https://openrouter.ai/nvidia/nemotron-nano-12b-v2-vl:free) |
| Best free for RAG/info gathering | [Owl Alpha](https://openrouter.ai/openrouter/owl-alpha) | [LFM2.5-1.2B-Thinking](https://openrouter.ai/liquid/lfm-2.5-1.2b-thinking:free) |
| Best free API outside OpenRouter | [Google AI Studio](https://aistudio.google.com/) (Gemini) | [Groq](https://console.groq.com/) (Llama-family speed) |
| Best free guardrail/auxiliary | [Nemotron 3.5 Content Safety](https://openrouter.ai/nvidia/nemotron-3.5-content-safety:free) | [Nemotron 3 Nano Omni](https://openrouter.ai/nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free) |

**Key caveat from the community:** Free models generally need more
specific prompting and aren't as reliable for fully autonomous agent
work as paid flagships. For unattended/agentic tasks, users report
needing detailed step-by-step instructions often drafted by a better
model (e.g., Claude, GPT 5.5).

All 26 free models at:
<https://openrouter.ai/models?max_price=0&input_modalities=text&supported_parameters=tools>
(18 text→text with tools; 26 total across all modalities).

------------------------------------------------------------------------

## Part 2: Tier 1 — Top Performers

Highest category rankings on OpenRouter.

| Model | Active Params | Context | Categories | Best For |
|----|----|----|----|----|
| **[Owl Alpha](https://openrouter.ai/openrouter/owl-alpha)** | — | 1.05M | Academia \#3, Finance \#5, Health \#8, Legal \#8 | Agentic workloads, long-context, tool use |
| **[Laguna M.1](https://openrouter.ai/poolside/laguna-m.1:free)** (Poolside) | — | 256K | Programming \#12, Science \#18, Technology \#27 | Complex coding, agentic software engineering |
| **[Nemotron 3 Super](https://openrouter.ai/nvidia/nemotron-3-super-120b-a12b:free)** (NVIDIA) | 12B / 120B | 1M | Finance \#23, Programming \#24, Academia \#34 | Multi-agent, long-context reasoning |
| **[gpt-oss-120b](https://openrouter.ai/openai/gpt-oss-120b:free)** (OpenAI) | 5.1B / 117B | 131K | SEO \#7, Finance \#21, Academia \#40 | Reasoning, agentic, production use |
| **[North Mini Code](https://openrouter.ai/cohere/north-mini-code:free)** (Cohere) | 3B / 30B | 256K | Programming \#18, Science \#45 | Agentic coding, terminal tasks, runs on consumer hardware |

------------------------------------------------------------------------

## Part 3: Tier 2 — Strong Alternatives

Solid performers with good community feedback.

| Model | Active Params | Context | Categories | Best For |
|----|----|----|----|----|
| **[Gemma 4 31B](https://openrouter.ai/google/gemma-4-31b-it:free)** (Google) | 30.7B dense | 256K | Roleplay \#28 | Multimodal (text+image+video), coding, 140+ languages |
| **[Gemma 4 26B A4B](https://openrouter.ai/google/gemma-4-26b-a4b-it:free)** (Google) | 3.8B / 25.2B | 256K | — | Near-31B quality at MoE efficiency, function calling |
| **[Hermes 3 405B](https://openrouter.ai/nousresearch/hermes-3-llama-3.1-405b:free)** (Nous) | 405B dense | 131K | — | Generalist, agentic, roleplay, Hermes-native |
| **[Llama 3.3 70B](https://openrouter.ai/meta-llama/llama-3.3-70b-instruct:free)** (Meta) | 70B dense | 131K | — | Multilingual dialogue, broad benchmarks |
| **[Qwen3 Next 80B A3B](https://openrouter.ai/qwen/qwen3-next-80b-a3b-instruct:free)** (Qwen) | 3B / 80B | 256K | — | RAG, tool use, agentic workflows, no thinking traces |
| **[Nemotron 3 Ultra](https://openrouter.ai/nvidia/nemotron-3-ultra-550b-a55b:free)** (NVIDIA) | 55B / 550B | 1M | Finance \#32 | Frontier reasoning, orchestration, coding agents |
| **DeepSeek V4 Flash** | — | — | — | Community favorite for speed; mixed reliability reports |

------------------------------------------------------------------------

## Part 4: Tier 3 — Specialized & Budget

Focused models for specific tasks.

| Model | Active Params | Context | Best For |
|----|----|----|----|
| **[Qwen3 Coder 480B](https://openrouter.ai/qwen/qwen3-coder:free)** (Qwen) | 35B / 480B | 1.05M | Agentic coding, function calling, repo-level reasoning |
| **[Laguna XS.2](https://openrouter.ai/poolside/laguna-xs.2:free)** (Poolside) | — | 256K | Efficient coding agent, compact footprint |
| **[Nemotron 3 Nano 30B](https://openrouter.ai/nvidia/nemotron-3-nano-30b-a3b:free)** (NVIDIA) | 3B / 30B | 256K | Specialized agentic AI, customization |
| **[Nemotron 3 Nano Omni](https://openrouter.ai/nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free)** (NVIDIA) | 3B / 30B | 256K | Multimodal perception sub-agent — text, image, video, audio |
| **[gpt-oss-20b](https://openrouter.ai/openai/gpt-oss-20b:free)** (OpenAI) | 3.6B / 21B | 131K | Consumer/GPU hardware, function calling, structured outputs |
| **[Nemotron Nano 12B V2 VL](https://openrouter.ai/nvidia/nemotron-nano-12b-v2-vl:free)** (NVIDIA) | 12B | 128K | Video understanding, document intelligence, OCR |
| **[Venice Uncensored](https://openrouter.ai/cognitivecomputations/dolphin-mistral-24b-venice-edition:free)** (Dolphin) | 24B | 32K | Uncensored/flexible roleplay and content |
| **[Llama 3.2 3B](https://openrouter.ai/meta-llama/llama-3.2-3b-instruct:free)** (Meta) | 3B | 131K | Lightweight, multilingual, dialogue |
| **[Nemotron Nano 9B V2](https://openrouter.ai/nvidia/nemotron-nano-9b-v2:free)** (NVIDIA) | 9B | 128K | Unified reasoning + non-reasoning, controllable thinking |
| **[LFM2.5-1.2B-Thinking](https://openrouter.ai/liquid/lfm-2.5-1.2b-thinking:free)** (LiquidAI) | 1.2B | 32K | Lightweight reasoning, agentic tasks, RAG, edge devices |
| **[LFM2.5-1.2B-Instruct](https://openrouter.ai/liquid/lfm-2.5-1.2b-instruct:free)** (LiquidAI) | 1.2B | 32K | Compact chat, edge inference, fast |
| **[Lyria 3 Pro Preview](https://openrouter.ai/google/lyria-3-pro-preview)** (Google) | — | 1M | Music generation — full songs, 48kHz, text+image→audio (\$0.08/song) |
| **[Lyria 3 Clip Preview](https://openrouter.ai/google/lyria-3-clip-preview)** (Google) | — | 1M | Music clips — 30s, 48kHz (\$0.04/clip) |
| **[Nemotron 3.5 Content Safety](https://openrouter.ai/nvidia/nemotron-3.5-content-safety:free)** (NVIDIA) | 4B | 128K | Guardrail model — moderates inputs AND outputs for LLMs/VLMs |

------------------------------------------------------------------------

## Part 5: Beyond OpenRouter — Other Free API Sources

Free model APIs exist outside of OpenRouter. Here's what's available
directly from providers:

| Provider | Free Tier | Notable Models | Rate Limits | Best For |
|----|----|----|----|----|
| **[Google AI Studio](https://aistudio.google.com/)** | Free tier | Gemini 2.5 Flash/Pro, Gemma | Generous (RPM/TPM) | Multimodal, long context, coding |
| **[Groq](https://console.groq.com/)** | Free tier | Llama 4, Mixtral, Gemma | Rate limited (RPM) | Speed — fastest inference available |
| **[Together AI](https://www.together.ai/)** | Free credits | Llama, Qwen, DeepSeek, Mixtral | Credit-based | Broad model selection, research |
| **[HuggingFace Inference](https://huggingface.co/inference-api)** | Free tier | Thousands of community models | Rate limited | Experimentation, niche models |
| **[NVIDIA NIM](https://build.nvidia.com/)** | Free credits | Full Nemotron family | Credit-based | Nemotron-native, enterprise grade |
| **[Cohere](https://cohere.com/)** | Free tier | North Mini Code, Command R | Limited RPM | Coding, RAG, embeddings |
| **[DeepSeek Platform](https://platform.deepseek.com/)** | Free tier | DeepSeek V4 Flash, V4 Pro | Generous | Cost-efficient coding and reasoning |

**Pro tip:** Several of these providers power the "free" models on
OpenRouter. Going direct can sometimes give you higher rate limits or
fresher model versions, but you lose OpenRouter's unified API and model
switching.

------------------------------------------------------------------------

## Part 6: Community Use Cases — What [r/hermesagent](https://www.reddit.com/r/hermesagent) Actually Uses

Based on threads from June 28-29, 2026.

### Information Gathering & Research

- **[Owl Alpha](https://openrouter.ai/openrouter/owl-alpha)** is the
  community's top free pick for info gathering. User AquaMoonTea: *"I
  mainly use owl-alpha for free. It does well as long as the prompt
  given is VERY specific."* Runs it in Docker for sandboxing, uses
  Claude to write detailed step-by-step prompts first. Switches to
  DeepSeek for anything important.
- Owl Alpha scores \#3 in Academia, \#5 in Finance, \#8 in Health/Legal
  — strong across knowledge domains.

### Coding & Development

- **[Laguna M.1](https://openrouter.ai/poolside/laguna-m.1:free)** ranks
  Programming \#12 on OpenRouter — the highest-ranked free coding model.
- **[Qwen3 Coder 480B](https://openrouter.ai/qwen/qwen3-coder:free)**
  offers 1M context for repo-level reasoning with 35B active params.
- **[North Mini
  Code](https://openrouter.ai/cohere/north-mini-code:free)** (Cohere) —
  Programming \#18, 3B active params runs on consumer hardware.
- Community member BatOk7254 on **Kimi 2.7 Coder**: *"Just does the
  work, no talking."*

### General Agent Tasks

- **DeepSeek V4 Flash** — most-mentioned free model in the community.
  Mixed reports:
  - Positive (YouAsk-IAnswer): *"I've been using Deepseek-v4-flash and
    it's been excellent for me."*
  - Negative (akgo, OP): *"It keeps on making a lot of mistakes
    consistently. Also, it starts lying and deleting wrong files."*
- **DeepSeek V4 Pro** — RepresentativeRuin75: *"Almost 300 million
  tokens last 5 days and \$3.88 total. Not a single problem."* (Note: V4
  Pro is paid, but extremely cheap.)

### The Prompt Quality Pattern

Multiple users independently arrived at the same workflow: use a strong
model (Claude, GPT 5.5) to write detailed step-by-step prompts, then
feed those to the free model. AquaMoonTea described this exactly;
RepresentativeRuin75 uses *"good prompts made by opus-4.8."*

### Sandboxing

AquaMoonTea: *"I also have software in a docker container to not have
them accidentally do things to my personal files. I already seen it
accidentally overwrite files a few times."* This is a recurring theme —
free models are more prone to filesystem mistakes, and Docker sandboxing
is a common mitigation.

### Other Models Mentioned

- **Mimo 2.5 / 2.5 Pro** — mentioned positively by BehindUAll
- **Kimi 2.7 Coder** — praised for directness
- Claude/Opus used as prompt-crafters for cheaper models
- Minimax M3 — discussed as potential option, unconfirmed by community

------------------------------------------------------------------------

## Part 7: Auxiliary & Specialized Use Cases

Free models aren't just for primary agent work. Several are
purpose-built for auxiliary roles:

### Image & Video Processing

- **[Nemotron Nano 12B V2
  VL](https://openrouter.ai/nvidia/nemotron-nano-12b-v2-vl:free)** —
  video understanding and document intelligence. Hybrid
  Transformer-Mamba, handles long-form video via Efficient Video
  Sampling. OCR, chart reasoning, multimodal comprehension. Scores ~74
  average across MMMU, MathVista, AI2D, OCRBench, ChartQA, DocVQA, and
  Video-MME.
- **[Nemotron 3 Nano
  Omni](https://openrouter.ai/nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free)**
  — designed as a *perception sub-agent* for enterprise agent systems.
  Accepts text, image, video, AND audio input — 2x throughput vs
  separate vision+speech pipelines. 300K context, 16K reasoning budget.
- **[Gemma 4 31B](https://openrouter.ai/google/gemma-4-31b-it:free) /
  [26B](https://openrouter.ai/google/gemma-4-26b-a4b-it:free)** — both
  support image and video input alongside text.

### Content Safety / Guardrails

- **[Nemotron 3.5 Content
  Safety](https://openrouter.ai/nvidia/nemotron-3.5-content-safety:free)**
  — the only free guardrail model listed. 4B params, multimodal
  (text+image). Moderates both inputs TO and responses FROM LLMs/VLMs.
  Fine-tuned from Gemma-3-4B. Use as a second-pass filter on sensitive
  outputs.

### Music & Media Generation

- **[Lyria 3](https://openrouter.ai/google/lyria-3-pro-preview)**
  (Google) — music generation via Gemini API. Pro: full songs at 48kHz.
  Clip: 30-second clips. Text+image→audio. Not a "free model" in the
  traditional sense (per-unit pricing) but listed on OpenRouter's free
  tier for the model API access.

### Lightweight / Edge Inference

- **[LFM2.5-1.2B-Thinking/Instruct](https://openrouter.ai/liquid/lfm-2.5-1.2b-instruct:free)**
  (LiquidAI) — 1.2B params, runs on edge devices. Thinking variant for
  reasoning/RAG tasks.
- **[Llama 3.2
  3B](https://openrouter.ai/meta-llama/llama-3.2-3b-instruct:free)** —
  multilingual, broad NLP, runs anywhere.
- **[gpt-oss-20b](https://openrouter.ai/openai/gpt-oss-20b:free)** —
  3.6B active params, designed for consumer/single-GPU hardware.

### The Free Models Router — OpenRouter's "Grab Bag"

OpenRouter provides
**[openrouter/free](https://openrouter.ai/openrouter/free)** — a router
that selects free models at random from the available pool. 200K
context, text+image→text. Use for low-stakes queries or variety; avoid
for anything requiring consistency.

### Provider Data Policies — Read Before You Use

Free models often come with data-use caveats: - **Owl Alpha:** *"Prompts
and completions may be logged by the provider and used to improve the
model."* - **[Laguna
XS.2](https://openrouter.ai/poolside/laguna-xs.2:free) /
[M.1](https://openrouter.ai/poolside/laguna-m.1:free) (Poolside):** *"If
you are using Laguna for free, we may use your inputs and outputs to
train and improve our models."* - **Most other free models:** Policies
vary — check provider docs.

**Rule of thumb:** Don't send anything sensitive through a free model
API unless you've verified zero data retention. For
personal/work-sensitive data, prefer local models or paid APIs with
explicit zero-retention guarantees.

------------------------------------------------------------------------

## Part 8: FAQ

1.  **Q: Which free model should I start with?**\
    **A:** DeepSeek V4 Flash (most-mentioned, fastest) or [Owl
    Alpha](https://openrouter.ai/openrouter/owl-alpha) (higher quality,
    needs specific prompts). Both free on OpenRouter.

2.  **Q: Can free models handle autonomous agent tasks?**\
    **A:** Mixed results. Community consensus: free models work for
    guided tasks with specific prompts, but struggle with fully
    autonomous work. Many users draft prompts with paid models first.

3.  **Q: What's the best free model for coding?**\
    **A:** [Laguna M.1](https://openrouter.ai/poolside/laguna-m.1:free)
    (Programming \#12 on OpenRouter) or [Qwen3 Coder
    480B](https://openrouter.ai/qwen/qwen3-coder:free) (1M context, 35B
    active).

4.  **Q: How do I prevent free models from messing up my files?**\
    **A:** Run them in Docker. Multiple community members report file
    overwrites with free models; sandboxing is the standard mitigation.

5.  **Q: Are free models good for image/video tasks?**\
    **A:** Yes. [Nemotron Nano 12B V2
    VL](https://openrouter.ai/nvidia/nemotron-nano-12b-v2-vl:free) and
    [Nemotron 3 Nano
    Omni](https://openrouter.ai/nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free)
    are purpose-built for multimodal perception. [Gemma 4
    31B](https://openrouter.ai/google/gemma-4-31b-it:free) also handles
    images and video.

6.  **Q: Can I use free models as a content safety filter?**\
    **A:** Yes — [Nemotron 3.5 Content
    Safety](https://openrouter.ai/nvidia/nemotron-3.5-content-safety:free)
    is a dedicated guardrail model, free on OpenRouter. Handles
    text+image moderation.

7.  **Q: What free APIs exist outside of OpenRouter?**\
    **A:** [Google AI Studio](https://aistudio.google.com/),
    [Groq](https://console.groq.com/), [Together
    AI](https://www.together.ai/), [HuggingFace
    Inference](https://huggingface.co/inference-api), [NVIDIA
    NIM](https://build.nvidia.com/), [Cohere](https://cohere.com/), and
    [DeepSeek Platform](https://platform.deepseek.com/) all offer free
    tiers.

8.  **Q: Should I use the OpenRouter Free Router?**\
    **A:** Only for low-stakes or variety-seeking queries. The model
    changes every request — no consistency guarantees.

9.  **Q: Are my prompts/data safe with free models?**\
    **A:** Varies by provider. Owl Alpha and Laguna explicitly state
    they may log and use data for training. Check each provider's policy
    before sending sensitive content.

10. **Q: What's the cheapest way to get reliable agent performance?**\
    **A:** Community pattern: use DeepSeek V4 Pro (extremely cheap —
    ~\$3.88 for 300M tokens over 5 days per one user report). Not free,
    but near-free for practical purposes.

------------------------------------------------------------------------

## Part 9: Knowledge Table

| Model | Provider | Active Params | Context | Modality | Top Category | Watch For |
|----|----|----|----|----|----|----|
| [Owl Alpha](https://openrouter.ai/openrouter/owl-alpha) | OpenRouter | — | 1.05M | text→text | Academia \#3 | Data logged, needs very specific prompts |
| [Nemotron 3 Ultra](https://openrouter.ai/nvidia/nemotron-3-ultra-550b-a55b:free) | NVIDIA | 55B/550B | 1M | text→text | Finance \#32 | Very large, may be slow |
| [North Mini Code](https://openrouter.ai/cohere/north-mini-code:free) | Cohere | 3B/30B | 256K | text→text | Programming \#18 | Coding-focused, not general-purpose |
| [Nemotron 3 Nano Omni](https://openrouter.ai/nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free) | NVIDIA | 3B/30B | 256K | multi→text | Translation \#36 | Auxiliary/sub-agent role |
| [Laguna XS.2](https://openrouter.ai/poolside/laguna-xs.2:free) | Poolside | — | 256K | text→text | Programming \#25 | Data logged on free tier |
| [Laguna M.1](https://openrouter.ai/poolside/laguna-m.1:free) | Poolside | — | 256K | text→text | Programming \#12 | Data logged on free tier |
| [Gemma 4 26B A4B](https://openrouter.ai/google/gemma-4-26b-a4b-it:free) | Google | 3.8B/25.2B | 256K | text+image+video→text | — | MoE, near-31B quality |
| [Gemma 4 31B](https://openrouter.ai/google/gemma-4-31b-it:free) | Google | 30.7B | 256K | text+image+video→text | Roleplay \#28 | Dense, resource-intensive |
| [Lyria 3 Pro Preview](https://openrouter.ai/google/lyria-3-pro-preview) | Google | — | 1M | text+image→audio | — | Music generation, per-song pricing |
| [Lyria 3 Clip Preview](https://openrouter.ai/google/lyria-3-clip-preview) | Google | — | 1M | text+image→audio | — | 30s clips, per-clip pricing |
| [Nemotron 3 Super](https://openrouter.ai/nvidia/nemotron-3-super-120b-a12b:free) | NVIDIA | 12B/120B | 1M | text→text | Finance \#23 | Hybrid Mamba-Transformer |
| [Free Router](https://openrouter.ai/openrouter/free) | OpenRouter | — | 200K | text+image→text | — | No consistency, random model |
| [LFM2.5-1.2B-Thinking](https://openrouter.ai/liquid/lfm-2.5-1.2b-thinking:free) | LiquidAI | 1.2B | 32K | text→text | — | Very small context, edge-only |
| [LFM2.5-1.2B-Instruct](https://openrouter.ai/liquid/lfm-2.5-1.2b-instruct:free) | LiquidAI | 1.2B | 32K | text→text | — | Very small context, edge-only |
| [Nemotron 3 Nano 30B](https://openrouter.ai/nvidia/nemotron-3-nano-30b-a3b:free) | NVIDIA | 3B/30B | 256K | text→text | — | Specialized agentic focus |
| [Nemotron Nano 12B V2 VL](https://openrouter.ai/nvidia/nemotron-nano-12b-v2-vl:free) | NVIDIA | 12B | 128K | multi→text | — | Vision/video specialist |
| [Qwen3 Next 80B A3B](https://openrouter.ai/qwen/qwen3-next-80b-a3b-instruct:free) | Qwen | 3B/80B | 256K | text→text | — | No thinking traces, fast |
| [Nemotron Nano 9B V2](https://openrouter.ai/nvidia/nemotron-nano-9b-v2:free) | NVIDIA | 9B | 128K | text→text | — | Unified reasoning+non-reasoning |
| [gpt-oss-120b](https://openrouter.ai/openai/gpt-oss-120b:free) | OpenAI | 5.1B/117B | 131K | text→text | SEO \#7 | Apache 2.0, single H100 |
| [gpt-oss-20b](https://openrouter.ai/openai/gpt-oss-20b:free) | OpenAI | 3.6B/21B | 131K | text→text | Finance \#50 | Consumer hardware focused |
| [Qwen3 Coder 480B](https://openrouter.ai/qwen/qwen3-coder:free) | Qwen | 35B/480B | 1.05M | text→text | — | Massive model, coding specialist |
| [Venice Uncensored](https://openrouter.ai/cognitivecomputations/dolphin-mistral-24b-venice-edition:free) | Dolphin | 24B | 32K | text→text | — | Uncensored, small context |
| [Llama 3.3 70B](https://openrouter.ai/meta-llama/llama-3.3-70b-instruct:free) | Meta | 70B | 131K | text→text | — | Older (Dec 2024), solid generalist |
| [Llama 3.2 3B](https://openrouter.ai/meta-llama/llama-3.2-3b-instruct:free) | Meta | 3B | 131K | text→text | — | Very small, basic tasks only |
| [Hermes 3 405B](https://openrouter.ai/nousresearch/hermes-3-llama-3.1-405b:free) | Nous | 405B | 131K | text→text | — | Namesake, 405B dense, older |
| [Nemotron 3.5 Content Safety](https://openrouter.ai/nvidia/nemotron-3.5-content-safety:free) | NVIDIA | 4B | 128K | multi→text | — | Guardrail only, not general-purpose |

------------------------------------------------------------------------

## Part 10: Sources & Threads

- [thread: "What models you are using with Hermes?", June 29
  2026](https://www.reddit.com/r/hermesagent/comments/1uiyufx/what_models_you_are_using_with_hermes/)
  — 14 comments, community model picks
- [thread: "Cheap/Free-Tier Model Use Case Examples", June 29
  2026](https://www.reddit.com/r/hermesagent/comments/1uiytee/cheapfreetier_model_use_case_examples/)
  — 5 comments, specific use cases and reliability feedback
- [OpenRouter free models
  listing](https://openrouter.ai/models?max_price=0&input_modalities=text&supported_parameters=tools)
  — model specs, category rankings, pricing
- [OpenRouter Rankings](https://openrouter.ai/rankings) — live usage and
  benchmark data
- [Google AI Studio](https://aistudio.google.com/),
  [Groq](https://console.groq.com/), [Together
  AI](https://www.together.ai/),
  [HuggingFace](https://huggingface.co/inference-api), [NVIDIA
  NIM](https://build.nvidia.com/), [Cohere](https://cohere.com/),
  [DeepSeek Platform](https://platform.deepseek.com/) — provider free
  tier documentation

------------------------------------------------------------------------

**Corrections or additions?** Open an issue or pull request in this repo. This is a
living resource — new free models appear regularly and rankings shift.

---

## Comment-Sourced Updates Added From Reddit

These notes came from the discussion under the original Reddit post. They are included here so the GitHub version reflects the thread, not just the original post.

### Practical caveat: free quotas can disappear quickly

A commenter reported that some free API keys, including NVIDIA and MiniMax examples, ran out very quickly under basic use. Treat free tiers as useful for testing, auxiliary tasks, light research agents, compaction, scheduled jobs, and experimentation — not as guaranteed daily-driver capacity.

### Why maintain this on GitHub

Community members specifically asked for a GitHub repo because:

- anyone can suggest edits via pull requests;
- maintainers can approve or reject changes;
- diffs make updates easier to review and summarize back to Reddit;
- the repo becomes another discovery/promotion channel;
- Markdown files can be edited locally, in Obsidian, in GitHub's web UI, or with any Git client.

### Suggested editing workflow from the comments

One suggested low-friction workflow:

1. Create a GitHub repo.
2. Keep each megathread as a Markdown file.
3. Optionally edit the files in Obsidian.
4. Use Obsidian Git, GitHub Desktop, the GitHub web editor, or the git CLI to sync changes.
5. Let community members submit pull requests for corrections and additions.

