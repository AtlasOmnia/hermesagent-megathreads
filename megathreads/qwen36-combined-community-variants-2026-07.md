# Qwen3.6 Community Variants — 27B (Dense) & 35B-A3B (MoE) Definitive Guide for Limited Local Hardware

LAST UPDATED: July 20, 2026
Combined refresh of the May 24, 2026 originals and the July 8 v2 posts, covering both Qwen3.6 local flagships + the NVFP4-MTP Blackwell frontier. This is a community resource, not a sales funnel. No benchmarks here are independent; all are publisher-reported unless explicitly noted (85 GPU-hour shootout by nathandreamfast).

GitHub mirror (permanent, Google-indexed): https://github.com/AtlasOmnia/hermesagent-megathreads/blob/main/megathreads/qwen36-combined-community-variants-2026-07.md
Original threads: 35B-A3B https://old.reddit.com/r/hermesagent/comments/1tmp2qy/ · 27B https://old.reddit.com/r/hermesagent/comments/1tn4lye/
See also: Free Models & APIs (https://old.reddit.com/r/hermesagent/comments/1uj9nkn/) · Model Civil War — local vs cloud vs hybrid (https://old.reddit.com/r/hermesagent/comments/1uqd00s/)

---

## ⚠️ START HERE — 27B vs 35B: Which base model for you?

**Read this table before anything below — it answers most questions.**

| Priority | Choose this | Why | Don't pick if |
|---|---|---|---|
| Best instruction-following / coding / agentic | **27B dense (Q4_K_M + MTP)** | SWE-bench Verified 77.2% (vs 35B's 73.4%); community 45-day test shows tighter instruction adherence | You need max tok/s — 27B is ~3.5× slower than 35B on same hardware |
| Fastest inference, math (AIME), low-VRAM | **35B-A3B MoE (Q6_K_XL)** | Only ~3B params active/token; ~18-20 tok/s on modest GPUs; AIME 2025 92.92, SWE-bench 73.4% | You're on Blackwell — NVFP4 versions give 88-93 tok/s on both, and NVFP4 27B gets AIME 92.7 |
| Blackwell, best speed + quality | **NVFP4 + MTP** (either base) | ~88-100 tok/s; near-FP8 quality | Your priority is coding benchmarks — NVFP4 drops 10% on SWE-bench (see NVFP4 honesty section) |
| Long-horizon agentic | Consider **Gemma 4 31B** instead | Community report: some prefer it over Qwen for long agent loops | You need Qwen-specific ecosystem |
| Newcomer to local inference | **DavidAU Heretic NEO-CODE 27B** | Uncensored + coding fine-tune, well-benchmarked per quant | You don't need uncensored use — use base Qwen 3.6-27B |
| Need vision (image input) | Skip MTP variants | MTP conflicts with vision on most builds; use base Unsloth MTP or HauhauCS with mmproj | You need MTP speed |

**Critical insight (multiple community reports):** The "template bug" is the single biggest factor in Qwen 3.6 tool-calling quality. Froggeric's patched chat templates fix most issues.

> "The worse tool calling is because of bugs in the standard chat template. On huggingface froggeric has some patched templates. I use those and since that I have had almost no issues with tool calling for qwen 3.6 35b a3b q4km." (u/i_am_me0_0, +47, r/LocalLLM)

**Template repo:** https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates

**The 95% agent success rate** (often cited) comes from 27B Q8 + **narrow tool schemas + froggeric templates**, NOT from running base model on default settings. With default settings, community reports ~75% success. Fix the template first, then pick your quant.

---

## 🔴 WHAT NOT TO PICK — Save yourself debugging time

**Do NOT use these for the following use cases:**

- **Coding agent (high-stakes):** ❌ Qwopus3.6-27B-Coder-MTP (67.0% SWE-bench) — this is 10% BELOW base 27B (77.2%). The reasoning traces improve structured workflows but regress on raw SWE-bench. Use 27B Q8 base or DavidAU NEO-CODE instead.
- **Agentic work on 35B Blackwell:** ❌ NVFP4 (50.2% SWE-bench) — this is 27% BELOW base 35B. Significant regression on coding benchmarks. Use unsloth UD Q6_K_XL (Qwen official) or Qvopus-style instead.
- **General coding without uncensored need:** ❌ HauhauCS Aggressive (no coding fine-tune) — pure safety removal means you're getting base behavior minus refusals. Use DavidAU NEO-CODE for actual coding improvements.
- **Reasoning-distilled on chat-heavy workloads:** ❌ lordx64 Opus 4.7 Distilled / rico03 Opus 4.6 / any distilled variant — these force `<think>` reasoning tokens even on simple questions, burning latency. Use base + reasoning OFF instead.
- **"Better than base" claims:** ❌ AEON Ultimate Uncensored (BF16) — publisher claims "measurably enhanced" capabilities. The 85 GPU-hour community shootout (nathandreamfast): **"Claims NOT supported by benchmark data."** Use only if you want NVFP4 uncensored and accept this caveat.
- **Beginner-first pick:** ❌ Any NVFP4 on non-Blackwell, reasoning-distilled on any hardware without understanding loops, HauhauCS uncensored when you just need base — these are all worse starting points than 27B Q4_K_M + base template.

---

## THE BASE MODELS

### Qwen3.6-35B-A3B (MoE)
- 35B total params, ~3B active (MoE: 256 experts, 8 routed + 1 shared per token)
- Gated DeltaNet + Hybrid Attention
- 262K native context (extensible to 1M with YaRN)
- Multimodal: yes (vision projector available)
- License: Apache 2.0
- **Qwen official benchmarks (base model):**
  | Benchmark | Score |
  |-----------|-------|
  | SWE-bench Verified | 73.4% |
  | SWE-bench Multilingual | 67.2% |
  | Terminal-Bench 2.0 | 51.5% |
  | MMLU-Pro | 85.2% |
  | GPQA Diamond | 86.0% |
  | **AIME 2026** | **92.7%** |
  | LiveCodeBench v6 | 80.4% |
- Official: https://huggingface.co/Qwen/Qwen3.6-35B-A3B

### Qwen3.6-27B (Dense)
- 27B dense params (all active, no MoE)
- Gated DeltaNet + Hybrid Attention
- 262K native context (extensible to 1M with YaRN)
- Multimodal: yes (vision projector available)
- License: Apache 2.0
- **Qwen official benchmarks (base model):**
  | Benchmark | Score |
  |-----------|-------|
  | **SWE-bench Verified** | **77.2%** |
  | SWE-bench Pro | 53.5% |
  | Terminal-Bench 2.0 | 59.3% |
  | MMLU-Pro | 86.2% |
  | GPQA Diamond | 87.8% |
  | AIME 2026 | 94.1% |
  | LiveCodeBench v6 | 83.9% |
- Official: https://huggingface.co/Qwen/Qwen3.6-27B

**Bottom line:** Both are strong. 27B dense wins coding (SWE-bench +4%); 35B MoE wins speed (3.5× faster on same VRAM). Math (AIME) is near-identical.

---

## VARIANT CATEGORIES — What changed from base

**1. UNCENSORING (lossless safety removal)**
Strip refusal behavior without touching capabilities. Same accuracy, fewer "I can't help with that." Expect 0-6/100 refusals in testing (vs 92-99/100 base).

**2. HERETIC (uncensor + capability-preserving surgical edits)**
Removes refusals AND extends thinking chains to compensate. More sophisticated than raw abliteration. **Best capability preservation in the latest testing** (see 85 GPU-hour shootout section).

**3. ABLITERATION (surgical refusal removal)**
Weight-space ablation targeting "refusal directions." Can shift capability/stability. Quality varies by method — see 85 GPU-hour shootout.

**4. REASONING DISTILLATION (from Claude Opus)**
Trained on chain-of-thought traces from Opus 4.6 or 4.7. Adds explicit `<think>...</think>` reasoning. **Known loop risk** (see KNOWN ISSUES) + burns latency on chat-heavy prompts even when not needed.

**5. MTP / SPECULATIVE DECODING (speed layer)**
Multi-token prediction — predicts 2-3 tokens per step instead of 1. ~1 GB overhead, ~1.5-2x speedup at greedy decoding (byte-for-byte verified by u/ElmBark).

**6. NVFP4 (Blackwell native, 4-bit float)**
NVIDIA's native 4-bit format. Near-FP8 quality at half the footprint. Blackwell-only (won't load on Ampere/Ada). **Known tradeoffs: see NVFP4 honesty section** — expect ~10% loss on SWE-bench for speed gains.

**7. AGENT-SPECIFIC FINETUNE**
Trained specifically for agentic tool-calling. New category: unsloth Qwen-AgentWorld addresses the looping/tool-leakage issues.

---

## THE VARIANTS

### 35B-A3B (MoE) — by type

#### UNCENSORED / ABLITERATED

---

**HauhauCS Aggressive** — 2,007,025 downloads, 2,923 likes (HF verified)
https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive

What: Base model with safety removed via abliteration; no capability changes. "Best lossless uncensored model" claim. K_P "Perfect" quants: custom format that preserves quality at slightly larger files.

Quality vs base (HF card verified):
- **Refusals:** 0/465 — near-complete safety removal confirmed
- **Claim:** "No changes to datasets or capabilities."
- **GGUF sizes (K_P quants):** Q4_K_M 21GB, IQ4_XS 19GB, Q6_K_P 31GB, Q8_0/Q8_K_P 44GB
- **mmproj file:** 899 MB (vision)
- ⚠️ **NO SWE-bench / AIME / benchmark delta listed in card.** "Lossless" claim is publisher-reported, not independently verified.

**Tradeoff:** Controversy remains (u/Wity_Mycologist_995 alleged theft; **unverified**). Highest-download uncensored pick, but provenance debates mean some users avoid it.

Best for: General uncensored use where you want the established proven default
VRAM: Q4_K_P 23GB | Q4_K_M 21GB | IQ4_XS 19GB
WHY PICK IT: 2M downloads = proven stable in production. 0/465 refusals confirmed. Settings: thinking general temp=1.0, coding temp=0.6, non-thinking temp=0.7.

Don't use if: You need coding improvement on top of uncensoring (use DavidAU NEO-CODE instead, even though that's 27B-only currently).

---

**llmfan46 heretic (35B-A3B and 27B)** — 35B: 110K DL | 27B: 64.8K DL
https://huggingface.co/llmfan46/Qwen3.6-35B-A3B-uncensored-heretic-GGUF
https://huggingface.co/llmfan46/Qwen3.6-27B-uncensored-heretic-v2-Native-MTP-Preserved-GGUF

What: Combination abliteration + decensor (MPOA method). Uses Heretic v1.3.0. Native MTP preserved in 27B version. NVFP4 variant released July 2026 (11.9K downloads).

**Quality vs base (HF card verified — best-preservation data in this guide):**
- **KL divergence: 0.0021** (vs 0.0469 for DavidAU Heretic — **22× closer to base behavior**)
- **Refusals: 6/100** (vs 92/100 original) — 94% fewer refusals
- **MMLU: 86.65% (original) → 85.67% (Heretic)** = 0.98% drop
- **No SWE-bench / AIME data listed** — capability preservation inferred from MMLU + KL
- **MPOA method:** "Surgical edits extend thinking chains rather than shorten them." (nathandreamfast)

**Best preservation of any abliterated variant in testing.** If you want uncensored + minimal quality loss, this is the data-backed pick. Pick DavidAU Heretic instead if you want the coding fine-tune on top.

Best for: Uncensored + MTP speed, or NVFP4 on Blackwell
VRAM: Q4_K_M ~18 GB (35B) | NVFP4-MTP ~18-20 GB on Blackwell
WHY PICK IT: **Lowest KL in any abliterated variant (0.0021)** — most faithful capability preservation. Operator field-tested for "pen testing" use cases.

---

**Huihui abliterated (35B-A3B NEW in July, 27B existing)** — 35B: 14.6K DL | 27B: 87.7K DL (MTP-GGUF variant)
https://huggingface.co/huihui-ai/Huihui-Qwen3.6-35B-A3B-abliterated (35B)
https://huggingface.co/huihui-ai/Huihui-Qwen3.6-27B-abliterated-MTP-GGUF (27B, MTP)

What: Pure abliteration using MPOA method. Simple, clean weight edits. Described as "crude, proof-of-concept to remove refusals" by publisher.

Quality vs base (85 GPU-hour shootout, nathandreamfast — only independent community test):
- **Tied for best** with llmfan46 heretic for capability preservation
- **Benchmark drops < 1% of base**
- MMLU preservation equivalent to llmfan46
- ⚠️ **No SWE-bench / AIME / KL data listed in card.** Inference from 85 GPU-hour testing.

OP note: "huihui did terribly comparing the Qwen 3.5 series. In this case with Qwen 3.6 27b it has redeemed itself."

Best for: Uncensored work with minimal quality loss
VRAM: Q6_K ~21 GB (27B)
WHY PICK IT: Community-tested best capability preservation. Clean, minimal weight edits. **If MPOA is what matters over KL-divergence math, this or llmfan46 — pick based on style preference.**

---

#### REASONING-DISTILLED

---

**lordx64 Opus 4.7 Distilled** — 29.9K downloads, 197 likes (HF verified, dataset public)
https://huggingface.co/lordx64/Qwen3.6-35B-A3B-Claude-4.7-Opus-Reasoning-Distilled

What: SFT on ~7,800 Claude Opus 4.7 reasoning traces with explicit `<think>...</think>` blocks. Attention-only LoRA (r=16, alpha=16, dropout=0.0). Trained 2 epochs / 978 steps on 4K token sequences, usable at 64K. Apache 2.0, weights + dataset public.

Quality vs base (HF card verified):
- **Training details public:** dataset lordx64/reasoning-distill-opus-4-7-max-sft (7.8K conversations)
- **Architecture:** 35B MoE, 256 experts, 8 routed, only ~3B active per token
- **Context:** 64K native; "routinely emits 5–30k tokens of `<think>` reasoning"
- **LoRA:** 3.44M params out of 35.1B (0.01%) — full base quality preserved on untouched dimensions
- **Intended use:** "Built for hard reasoning: graduate-level STEM, competition math (AIME / MATH), code reasoning with explicit walk-through, multi-step logic puzzles, and agentic planning"
- **GGUF sizes:** IQ4_XS 18.9 GB, Q5_K_M ~25 GB, Q8_0 ~35 GB
- **Eval published (not SWE-bench):**
  | Benchmark | Score |
  |-----------|-------|
  | GSM8K CoT (8-shot, flexible-extract) | 84.3% |
  | GSM8K CoT (8-shot, strict-match) | 76.7% |
  | MMLU-Pro (5-shot) | 74.9% |
  | AIME 2024/2025 | pending extraction |
  | SWE-bench Verified | no benchmark listed |

**Tradeoffs:** Loop risk on chat-heavy workloads. Community: "Almost every such finetuned 35B model loops." Set reasoning budget to max 4096 tokens. Uses thinking tokens even on simple questions → burns latency on chat.

Best for: Hard reasoning tasks, grad-level STEM, competition math
VRAM: IQ4_XS 18.9 GB
WHY PICK IT: **Most transparent training card** in the lineup — dataset + hyperparameters public. Community endorsement (+205 upvotes, r/LLMStudio) for Opus reasoning in a 35B MoE picking up "Opus price, tiny active-param cost." Pick this over HauhauCS or DavidAU if you want structured reasoning AND can tolerate the looping footgun.

Don't use if: Your workload is mostly chat or simple Q&A — you'll burn 3-30K thinking tokens per reply for no benefit.

---

**hesamation Opus 4.6 Distilled** — 205.9K downloads, 266 likes
https://huggingface.co/hesamation/Qwen3.6-35B-A3B-Claude-4.6-Opus-Reasoning-Distilled-GGUF

What: Jackrong-inspired recipe on Opus 4.6 traces. Uses nohurry Opus 4.6 reasoning dataset + Jackrong Qwen3.5 recipe + ReFT.

Quality vs base: Higher download count than lordx64 = broader adoption. Same looping risk applies. ⚠️ No card-level benchmarks listed.

Best for: Reasoning with older Opus 4.6 style
VRAM: Q4_K_M ~19 GB
WHY PICK IT: Jackrong recipe is proven. Preference for 4.6 reasoning traces over 4.7.

---

**AEON Ultimate Uncensored** — 35B NVFP4: no downloads (separate repo) | 27B BF16: 18,340 downloads, 135 likes
https://huggingface.co/AEON-7/Qwen3.6-27B-AEON-Ultimate-Uncensored-BF16

What: "Lossless abliteration." Publisher claims capabilities "measurably enhanced" (not just preserved). Zero refusal claim on 100-prompt test.

Quality vs base (85 GPU-hour community shootout):
- ⚠️ **"AEON's enhanced capability claims were not supported by the benchmark data."** (nathandreamfast)
- MTP acceptance: mean 3.3/3 tokens accepted; ~90% P0 acceptance. Fast on Blackwell.
- DFlash on this repo beats MTP +26% median, +52% peak on DGX Spark (per card).
- Speed numbers (RTX PRO 6000 Blackwell): NVFP4 variants 96-118 tok/s.
- ⚠️ **NO SWE-bench / AIME / benchmark delta listed.** Don't buy into "better than base" hype.

Best for: Blackwell NVFP4 + uncensored where speed matters and you accept the capability claim caveat
VRAM: NVFP4 ~26 GB on Blackwell (BF16 base is 52 GB)
WHY PICK IT: Blackwell-specific use case. NVFP4 speed on an uncensored variant. **Don't pick this expecting better-than-base capability.**

---

**DavidAU 40B Opus-Deckard Heretic** — 16.7K downloads, 135 likes (NEW July 2026)
https://huggingface.co/DavidAU/Qwen3.6-40B-Claude-4.6-Opus-Deckard-Heretic-Uncensored-Thinking-NEO-CODE-Di-IMatrix-MAX-GGUF

What: Expanded from 27B → 40B. Trained on Claude 4.6 Opus + Deckard dataset + Heretic uncensored + NEO-CODE finetune. Multiple stages.

**Quality claims (HF card — Nightmedia benchmarks, mxfp8):**
| Benchmark | Qwen3.6-27B base | Qwen3.6-35B-A3B base | This 40B |
|---|---|---|---|
| arc-c | 0.647 | 0.581 | **0.711** |
| arc-e | 0.803 | 0.757 | **0.879** |
| boolq | 0.910 | 0.892 | 0.910 |
| hswag | 0.773 | 0.751 | **0.790** |
| obkqa | 0.450 | 0.428 | **0.514** |
| piqa | 0.806 | 0.803 | **0.823** |
| wino | 0.742 | 0.688 | 0.763 |

Claims "exceeds 6/7 benchmarks vs base 27B, exceeds all 7 for 35B-A3B."

- Publisher claim: "First model this size to breach 700 ARC-C in both 8-bit and 4-bit."
- ⚠️ **NO SWE-bench / AIME / community independent verification.** In-house (Nightmedia) numbers only.
- GGUF: Q4_K_M ~16 GB

Community reaction (r/LocalLLaMA, mixed):
- u/Murflaw7424: "this model slaps. For document analysis, tool calls have not skipped a beat... LESS verbose and less prone to getting stuck in a loop."
- u/llama-impersonator (+15): "davidau has always produced useless schizo models... makes the model much more stupid."
- u/boyobob55 (+5): "basically they removed the safety filter via heretic method AND it's been finetuned on Claude Opus 4.6 reasoning... worked really well for me."

Best for: Document analysis power users willing to experiment
VRAM: Q4_K_M ~24 GB (larger base)
WHY PICK IT: Polarizing — works well for some, poorly for others. Try if 27B DavidAU isn't cutting it.

---

#### AGENT-SPECIFIC (NEW category, July 2026)

---

**unsloth Qwen-AgentWorld-35B-A3B** — 678,322 downloads, 196 likes
https://huggingface.co/unsloth/Qwen-AgentWorld-35B-A3B-GGUF

What: Agent-tuned 35B MoE from Unsloth (the default MTP reference author). Tuned for agentic tool-calling workflows.

Quality vs base: ⚠️ **Publisher claims "addresses looping/tool-leakage issues" but NO independent benchmarks published yet.** Quality delta vs base is unknown.

Community: "unsloth/Qwen-AgentWorld-35B-A3B-GGUF, definitely suits my needs with a proper agentic coder." (u/dsdt) + u/TTVDminx: "Use a higher Quant level if you can. Otherwise maybe look into the new model they dropped."

Fixes: Designed to address the looping + tool-leakage issues that plague heretic/distilled variants on long agentic runs.

Best for: **Agents that need to run long chains** — coding agents, research agents, Hermes workflows
VRAM: Q4_K_M ~18 GB | Q5_K_M ~22 GB
WHY PICK IT: Unsloth's pedigree means high likelihood this actually works. Pick if heretic variants give you tool-leakage.

**Caveat:** Brand-new category. Independent benchmarks not yet published. Pick this only after testing — not as default.

---

**Jackrong Qwopus3.6-35B-A3B-v1** — 2,943 downloads, 60 likes
https://huggingface.co/Jackrong/Qwopus3.6-35B-A3B-v1

What: Early MoE fine-tune on Qwen3.6-35B-A3B (bf16 35.95B params). Three-stage curriculum SFT on Claude Opus 4.7 + 4.6 distillation. "An early Qwopus-style exploration on 35B MoE" per publisher.

Quality vs base: ⚠️ **No benchmark listed.** The 27B Qwopus v2 (90.9K downloads) is the one with published MTP benchmarks; this 35B A3B v1 has no published speed/quality data.

Loop risk: Same as other reasoning-distilled variants. Use reasoning budget 4096 max.

**MTP: NOT yet available.** "Qwopus MTP: Requested, not delivered." Will be added when ready.

Best for: Reasoning + coding on MoE (when MTP arrives)
VRAM: Q4_K_M ~19 GB (estimated)
WHY PICK IT: Opus distill recipe on 35B-A3B. **Currently underperforms unsloth AgentWorld + llmfan46 heretic since no MTP yet and no benchmarks.** Wait for MTP or independent eval before adopting.

---

#### MTP / SPEED

---

**unsloth MTP reference (35B-A3B)** — 609,978 downloads, 744 likes (HF verified)
https://huggingface.co/unsloth/Qwen3.6-35B-A3B-MTP-GGUF

What: The de-facto MTP reference from Unsloth. UD-Q4_K_XL (22.66 GB) recommended.

Quality vs base: Zero quality loss at greedy decoding. Byte-for-byte verified by u/ElmBark ("ran all four tasks in all three modes and diffed the outputs, they match byte for byte") and u/emprahsFury: "all speculative decoding mechanisms include a verification step."

**Note:** Card lists **base model benchmarks** (SWE-bench 73.4%, Terminal-Bench 51.5), NOT deltas vs MTP. MTP preservation claim is based on greedy decoding verification, not benchmark deltas.

Speed gains: ~1.5-2× tok/s over no-MTP. ~1 GB overhead.

Requirements: Custom llama.cpp build with PR #22673, or Unsloth Studio (bundled), or havenoammo's Docker images.

Limitations:
- `-np > 1` (parallel decoding) not supported
- `--mmproj` (vision) not supported with MTP on some builds

Best for: Generic "give me faster inference with zero quality loss"
VRAM: UD-Q4_K_XL ~22.66 GB (full)
WHY PICK IT: The **default** MTP pick. If you're picking a 35B-A3B + MTP variant, start here.

---

**michaelw9999 35B-A3B NVFP4-MTP** — 166,343 downloads, 6 likes (HF verified)
https://huggingface.co/michaelw9999/Qwen3.6-35B-A3B-NVFP4-MTP-GGUF

What: NVFP4 + MTP. Blackwell-only. Operator uses 27B sibling as daily driver; spot-tests this 35B MoE version.

Quality vs base: ⚠️ Same byte-for-byte guarantee at greedy decoding as unsloth MTP. "Near-FP8 at NVFP4" claim per publisher. Community pushback (u/Remove_Ayys): "Advertising W4A16 → W4A4 as 'without any accuracy degradation' is disingenuous. Benchmarks you selected are simply not sensitive to the change."

**Use NVFP4 because it runs on your Blackwell — not because someone claimed zero loss.**

Speed: u/nathandreamfast: "Q8 NVFP4, with MTP I get about 100 tps on the 5090."

Requirements: Blackwell GPU. **Will NOT load on Ampere (30xx) or Ada (40xx).**

Best for: Blackwell users wanting max MoE speed
VRAM: ~18 GB + KV cache + MTP head

WHY PICK IT: Fastest proven MoE config on 27B-class Blackwell cards. Not for Ampere/Ada.

---

**DFlash speculative decoding (35B-A3B)** — 58.6K downloads, 225 likes
https://huggingface.co/z-lab/Qwen3.6-35B-A3B-DFlash

What: Separate 4-layer block-diffusion draft model for speculative decoding. Not a chat model — it's a speedup overlay on base Qwen3.6.

Quality vs base: "2-3x theoretical, but community reports variable — 145-450 t/s on RTX 6000" (May thread). u/1uyay0w: "DFlash drafts 15 tokens in a row, flies through repetitive or structured stuff where long runs actually stick, like JSON (152 tok/s, 3.4x). On creative text most guesses are wrong... wastes the work and can dip below baseline, 42 vs 44."

Key distinction: "MTP only guesses 3 in parallel from inside the model, so wrong guess costs almost nothing and never drops below baseline."

Best for: Structured output, coding, JSON generation
VRAM: 0.88 GB for the drafter (on top of base)
WHY PICK IT: If you're doing coding/structured work and want max speed. DFlash **dies with offload** — only pays off when model fully in VRAM. For chat/creative, use MTP.

---

### 27B (Dense) — by type

#### UNCENSORED / ABLITERATED

---

**HauhauCS Aggressive (27B)** — 358,952 downloads, 527 likes (HF verified)
https://huggingface.co/HauhauCS/Qwen3.6-27B-Uncensored-HauhauCS-Aggressive

What: Base Qwen3.6-27B with abliterated refusals. 0/465 refusals claim. "Best lossless uncensored model." No capability changes per publisher.

Quality vs base (HF card verified):
- **Refusals:** 0/465 near-complete safety removal
- **GGUF sizes (K_P quants):** Q4_K_M ~18 GB, IQ4_XS ~14 GB, Q6_K_P ~27 GB, Q8_K_P ~37 GB
- ⚠️ **NO SWE-bench / AIME / benchmark delta listed in card.** "Lossless" claim is publisher-reported.

85 GPU-hour community test: "performed decently, but tooling/provenance concerns make it less reliable." Controversy: u/Wity_Mycologist_995 alleged theft (unverified).

Best for: General uncensored use on 27B dense
VRAM: Q4_K_M ~18 GB | IQ4_XS ~14 GB
WHY PICK IT: 359K downloads = established in production. 0/465 refusals confirmed. **Community prefers Huihui or llmfan46 heretic for verified capability preservation** (both have data-backed claims).

Settings: thinking general temp=1.0, coding temp=0.6, non-thinking temp=0.7.

---

**llmfan46 heretic (27B)** — 62,155+ downloads, 157 likes (HF verified)
https://huggingface.co/llmfan46/Qwen3.6-27B-uncensored-heretic-v2-Native-MTP-Preserved-GGUF

What: Combination abliteration + decensor (MPOA). 94% fewer refusals. Native MTP preserved. NVFP4 MTP version available for Blackwell.

**Quality vs base (HF card verified):**
- **KL divergence: 0.0021** (vs 0.0469 for DavidAU Heretic — 22× closer to base)
- **Refusals: 6/100** (vs 92/100 original) — 94% fewer
- **MMLU: 86.65% (original) → 85.67% (Heretic)** = 0.98% drop
- **MPOA method:** "Surgical edits extend thinking chains rather than shorten them." (nathandreamfast)
- ⚠️ **NO SWE-bench / AIME listed.** Inferred preservation only.

Best for: Uncensored with maximum fidelity + MTP speed
VRAM: Q4_K_M ~16 GB | NVFP4-MTP ~18 GB on Blackwell
WHY PICK IT: **Lowest KL of any abliterated variant = best-preserved base behavior.**

---

**Huihui abliterated (27B)** — 87.7K downloads (MTP-GGUF variant), 80 likes
https://huggingface.co/huihui-ai/Huihui-Qwen3.6-27B-abliterated-MTP-GGUF

What: Pure abliteration using MPOA method. "Crude, proof-of-concept to remove refusals" (publisher). Simple, clean weight edits. MTP-enabled GGUF form.

Quality vs base (85 GPU-hour shootout, nathandreamfast):
- **Tied for best** with llmfan46 heretic. "Preserved model capability best overall with smallest benchmark drops."
- Capability preserved within 1% of base.
- ⚠️ **No SWE-bench / AIME / KL listed in card.** Inference from community testing.

Best for: Uncensored with minimal quality loss
VRAM: Q4_K_M ~16 GB | Q6_K ~21 GB (tighter at 24 GB)
WHY PICK IT: Community-tested best capability preservation. **Clean, minimal weight edits. MTP included in GGUF variant.**

---

#### HERETIC + FINE-TUNE

---

**DavidAU Heretic Uncensored NEO-CODE** — 131,380 downloads, 402 likes (HF verified)
https://huggingface.co/DavidAU/Qwen3.6-27B-Heretic-Uncensored-FINETUNE-NEO-CODE-Di-IMatrix-MAX-GGUF

What: Two-stage: (1) heretic uncensor (4/100 refusals, down from 99/100 base), (2) coding fine-tune via Unsloth.

**Quality vs base (HF card verified):**
- **KL divergence (Heretic):** 0.0469 ("less than 0.3 is great, lower is excellent")
- **Refusals:** 4/100 (vs 99/100 base)
- **Per-quant quality table:**
  | Quant | Same Top P vs BF16 | Mean KLD |
  |---|---|---|
  | IQ2_M | 82.82% | 0.1556 |
  | IQ3_M | 89.76% | 0.0569 |
  | IQ4_XS | 94.14% | 0.0172 |
  | Q4_K_M | 94.51% | 0.0147 |
  | Q6_K | 97.41% | 0.0024 |
  | Q8_0 | 98.47% | 0.0013 |

**In-house benchmarks (Nightmedia, instruct mode, mxfp8):**
| Benchmark | Qwen3.6-27B base | This model | Delta |
|---|---|---|---|
| arc-c | 0.647 | **0.673** | +4.0% |
| arc-e | 0.803 | **0.846** | +5.4% |
| boolq | 0.910 | 0.905 | -0.5% |
| hswag | 0.773 | — | — |
| obkqa | 0.450 | — | — |
| piqa | 0.806 | — | — |
| wino | 0.742 | — | — |

Note: Some benchmark columns not reported. The arc-c/arc-e improvements are real but the "exceeds root performance" publisher claim is stronger than independently verified — only arc-c and arc-e show gains, boolq shows a small loss.

**Quant guidance (publisher):** Q4_K_M = 94% of full BF16 precision. IQ2_M = 83%.

Best for: **Coding** default (for uncensored users)
VRAM: Q4_K_M ~15.7 GB (~19 GB with context)
WHY PICK IT: Verified KL + quant tables let you pick your VRAM budget with known quality cost. 4/100 refusals means effectively uncensored. The arc-c gain (+4%) + coding fine-tune make it the community default. **This is NOT an improvement across all benchmarks** — only arc-c and arc-e show confirmed gains; treat "exceeds base" claim as marketing.

Settings: thinking temp=1.0, coding temp=0.6, instruct temp=0.7.

---

**DavidAU NEO-CODE (non-uncensored)** — 4,552 downloads, 69 likes
https://huggingface.co/DavidAU/Qwen3.6-27B-NEO-CODE-Di-IMatrix-MAX-GGUF

What: Same coding fine-tune WITHOUT heretic uncensoring. Pure capability addition.

Quality vs base: Same arc-c/arc-e improvements as uncensored version. No capability preservation concerns because it's an addition, not a safety modification.

Best for: Users who want coding gains without removing refusals
VRAM: Q4_K_M ~15.7 GB
WHY PICK IT: If you want NEO-CODE but your use case doesn't need uncensored.

---

#### REASONING-DISTILLED

---

**rico03 Opus 4.6 Distilled (27B)** — 7,386 downloads, 48 likes (HF verified)
https://huggingface.co/rico03/Qwen3.6-27B-Claude-Opus-Reasoning-Distilled-GGUF

What: SFT on ~14K Claude 4.6 Opus reasoning traces using Jackrong's methodology. Structured `<think>...</think>` blocks. Apache 2.0.

**Quality vs base (HF card verified):**
- **Training data:** ~14K traces from nohurry 3000x Opus 4.6 + Roman111111 10K Opus dataset
- **Methodology:** Jackrong recipe adapted for Qwen3.6-27B
- **GGUF sizes:** Q2_K ~10GB, Q3_K_M ~13GB, Q4_K_M 16.5GB, Q5_K_M ~19GB, Q6_K ~22GB, Q8_0 28.6GB
- **Settings:** thinking temp=1.0, coding temp=0.6, instruct temp=0.7
- ⚠️ **"Base model benchmarks" listed (77.2 SWE-bench, 94.1 AIME 2026, etc.) — but these are BASE Qwen3.6-27B numbers, NOT the distilled variant.** Card does not provide delta showing how distilled compares to base. Train loss 0.305 reported.
- SWE-bench / AIME / benchmark delta vs base: **NOT listed.**

Best for: Reasoning on 27B dense
VRAM: Q4_K_M 16.5GB (~20GB with context)
WHY PICK IT: Opus 4.6 reasoning on 27B-dense (community's preferred dense base). ⚠️ **Independent quality delta vs base is NOT published — you're trusting the "Opus 4.6 reasoning traces" training.**

---

**Brian6145 Opus + Sonnet Distilled NVFP4+MTP (27B)** — 22.4K downloads, 28 likes
https://huggingface.co/Brian6145/Qwen3.6-27B-Claude-Opus-DeepSeek-Distilled-Imatrix-MTP-GGUF

What: Multi-teacher distillation from Claude Opus + Sonnet. NVFP4 + MTP. vLLM-optimized.

Quality vs base: Less affected by deep-thinking loop (reasoning-distilled variants trained on flattened traces). ⚠️ ⚠️ **⚠️ No benchmarks listed.**

Best for: Blackwell users wanting Opus reasoning + MTP speed + NVFP4 efficiency
VRAM: NVFP4 ~16 GB on Blackwell
WHY PICK IT: Blackwell all-in-one (reasoning + speed + quant). Only if you're Blackwell.

---

**Jackrong Qwopus3.6-27B-v2-MTP-GGUF** — 90,956 downloads, 378 likes (HF verified)
https://huggingface.co/Jackrong/Qwopus3.6-27B-v2-MTP-GGUF

What: v2 update. Reasoning-tuned with MTP. "Trace Inversion & Negentropy" training. Focus: agentic coding, DevOps, structured logic, math. Fine-tuned via Unsloth + hardware engineer Kyle Hessling.

Quality vs base (HF card verified — 30-question benchmark):
- **Speed:** 10.46 T/s vs 6.29 T/s (Qwen3.6-27B base) = **1.66× faster**
- **Latency savings:** 14,902s → 6,488s total (56.5% time reduction)
- **Token efficiency:** -27.7% completion tokens (more compact responses)
- **Coverage:** 30/30 benchmark prompts

**Domain-level breakdown:**
| Domain | MTP speedup | Time saved |
|---|---|---|
| Logic | 2.31× | 38.5 min → 16.7 min |
| Coding | 2.25× | 1.52 hrs → 40.6 min |
| DevOps | 2.31× | 47.4 min → 20.5 min |
| Math | 2.35× | 1.01 hrs → 25.8 min |
| Edge | 2.27× | 10.3 min → 4.5 min |

**GGUF sizes:** MTPQ3_K_M 13.5 GB, MTPQ4_K_M 16.8 GB, MTPQ6_K 22.4 GB, MTPQ8_0 29 GB.

Best for: Agents + coding on 27B with verified MTP speed gains
VRAM: MTPQ4_K_M ~16.8 GB
WHY PICK IT: HF-verified MTP speed = double signal on top of r/LocalLLaMA community "best Q4 for agent/coding" claim. 2.35× math speedup, 2.25× coding speedup, more compact output than base. **Pick this for reasoning + MTP on 27B.**

---

**Jackrong Qwopus3.6-27B-Coder-MTP-GGUF** — 125,312 downloads, 330 likes (HF verified)
https://huggingface.co/Jackrong/Qwopus3.6-27B-Coder-MTP-GGUF

What: Reasoning-enhanced agentic coding model with MTP. Based on Qwopus3.6-27B-v2.

**Quality vs base (HF card verified):**
- **SWE-bench Verified (thinking-off, Q5_K_M GGUF): 67.0% (335/500 resolved)**
- ⚠️ **This is a REGRESSION: 10.2% BELOW base 27B (77.2%).** The reasoning traces improve structured workflows but hurt raw SWE-bench.
- Repository breakdown: scikit-learn 84%, xarray 82%, requests 75%, django 72%, sympy 64%, pytest 63%, sphinx-doc 59%, matplotlib 59%
- Speed: "~100 t/s on RTX 5090 with MTP" (card claim)
- GGUF sizes: Q4_K_M ~15 GB, Q5_K_M ~20 GB (recommended), Q8_0 ~29 GB

**⚠️ DO NOT use for raw SWE-bench performance** — you'll lose 10% vs base. Use this variant if you have specific agentic coding workflows where reasoning traces improve structured output.

Best for: Agentic coding workflows where structured reasoning > raw SWE-bench
VRAM: Q5_K_M ~20 GB
WHY PICK IT: Only if you want reasoning-enhanced coding output + MTP speed + accept the 10% SWE-bench regression. Closes the "no Qwopus Coder MTP" gap from May thread, but with a clear trade.

---

#### MTP / SPEED

---

**unsloth MTP reference (27B)** — 2,860,615 downloads, 1,131 likes (HF verified)
https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF

What: The de-facto MTP reference from Unsloth. Q4_K_M 17.11 GB recommended.

Quality vs base: Zero quality loss at greedy decoding (byte-for-byte verified by u/ElmBark).

Speed: ~1.5-2× over no-MTP. ~1 GB overhead.

Requirements: Custom llama.cpp with PR #22673.

Best for: Default fastest inference pick
VRAM: Q4_K_M 17.11 GB (~20 GB with context)
WHY PICK IT: The default MTP for 27B.

---

**michaelw9999 27B NVFP4-MTP** — 88,404 downloads, 48 likes (HF verified, operator daily driver)
https://huggingface.co/michaelw9999/Qwen3.6-27B-NVFP4-MTP-GGUF

What: NVFP4 + MTP. Blackwell-only. Operator's daily driver.

Quality vs base: Same byte-for-byte guarantee at greedy decoding. "Near-FP8 at NVFP4" per publisher. Community pushback (u/Remove_Ayys): "disingenuous — benchmarks not sensitive enough to show the change."

Speed: ~1.7-2× over no-MTP. ~2× tok/s at 16.19 GB.

Real config (u/notheresnolight): "Qwen3.6 27B MTP at Q6_K with 200K context runs faster on my 5090 than Sonnet 5. I get around 100-120 t/s."

**NVIDIA card verified (BF16 vs NVFP4):**
| Benchmark | BF16 (FP8 baseline) | NVFP4 | Delta |
|---|---|---|---|
| MMLU Pro | 86.1 | **86.3** | +0.2 |
| GPQA Diamond | 86.0 | 85.5 | -0.5 |
| AIME 2025 | 93.1 | **92.7** | -0.4 |
| τ²-Bench Telecom | 95.2 | **95.4** | +0.2 |
| HLE | 21.7 | **21.8** | +0.1 |
| SciCode | 44.8 | 44.5 | -0.3 |

Small deltas in both directions; "lossless" claim holds for these benchmarks, but SWE-bench not reported by NVIDIA card.

Best for: Blackwell users wanting max speed + biggest dense model that fits 24GB
VRAM: 16.19 GB + KV cache + MTP head
WHY PICK IT: Operator's daily driver. Largest dense model fitting 24GB Blackwell with context room. Not for Ampere/Ada.

---

**Qwen3.6-27B + DFlash** — See DFlash in 35B section. Same drafter works.
https://huggingface.co/z-lab/Qwen3.6-27B-DFlash

Best for: Structured output, coding. Same caveats as 35B.

---

## 🔴 THE NVFP4-MTP FRONTIER — Honest take (Blackwell only)

NVFP4 is NVIDIA's native 4-bit float format. On Blackwell (RTX 5090, B-series) it gives near-FP8 quality at roughly half the footprint of FP8. Combine with MTP and you get 35B MoE or 27B dense running fast on single 24GB cards.

**Requirements:**
- Blackwell-class GPU (RTX 5090, B200, etc.). **Will NOT load on Ampere (30xx) or Ada (40xx).**
- Backend with NVFP4 + MTP support (current llama.cpp / LM Studio with Blackwell build).

### 🔴 NVFP4 Honesty — read this before picking

Publisher claims are often "near-lossless." Reality: **NVFP4 is a quantization.** It trades quality for size. The tradeoffs exist and vary by benchmark.

**RedHatAI NVFP4 for 35B-A3B (most comprehensive public benchmark):**
| Benchmark | BF16 base | NVFP4 | Recovery % |
|---|---|---|---|
| GSM8k Platinum (0-shot) | 95.73 | 96.08 | 100.37% |
| IfEval (0-shot) | 93.09 | 92.45 | 99.31% |
| **AIME 2025** | **92.92** | **91.25** | **98.21%** |
| GPQA Diamond | 84.51 | 84.68 | 100.20% |
| Math 500 | 84.80 | 85.00 | 100.24% |
| **LCB Codegen V6** | **77.33** | **74.67** | **96.55%** |
| MMLU Pro Chat | 85.32 | 84.70 | 99.28% |
| **SWE-bench Verified** | **54.8** | **50.2** | **91.61%** |
| BFCLv4 Overall | 57.83 | 56.10 | 97.01% |
| BFCLv4 Single Turn | 53.81 | 53.45 | 99.34% |
| BFCLv4 Multi-Turn | 62.25 | 58.13 | 93.38% |
| BFCLv4 Agentic | 49.91 | 49.31 | 98.80% |

**Key insight: NVFP4 loses 8.4% on SWE-bench Verified and 4.6% on LCB Codegen — the two most important agentic coding benchmarks.** Math and instruction-following stay strong (AIME 98.2%, MMLU 99.3%).

**Why this matters:** If your primary use case is agentic coding, **don't pick NVFP4 35B expecting parity with BF16.** Use unsloth UD-Q6_K_XL (Qwen official) at the same VRAM footprint for better coding retention.

**Use NVFP4 when:**
- You need fastest inference on Blackwell and SWE-bench isn't your priority
- You're running chat / general reasoning / instruction-following (AIME/MMLU hold up)
- You want the largest model fitting 24GB

**Don't use NVFP4 when:**
- SWE-bench / agentic coding is your primary use case
- You're on Ampere/Ada (files won't load)
- You want mathematically-lossless quantization (no such thing — use Q8_0 instead)

### Verified NVFP4-MTP family (downloads as of July 19)

**35B-A3B NVFP4:**
| Variant | Downloads | Author |
|---|---|---|
| nvidia 35B-A3B NVFP4 (reference) | 8,515,848 | NVIDIA |
| RedHatAI 35B-A3B NVFP4 | 1,926,682 | RedHatAI |
| michaelw9999 35B-A3B NVFP4-MTP | 166,343 | michaelw9999 |
| Jackrong Qwopus3.6-35B-A3B-Coder-MTP | 500,846 | Jackrong |
| AEON-7 35B-A3B heretic NVFP4 | 245K | AEON-7 |
| HauhauCS 35B-A3B Aggressive NVFP4 | 90K | lyf |
| sakamakismile 35B-A3B NVFP4 | 32.5K | sakamakismile |

**27B NVFP4:**
| Variant | Downloads | Author |
|---|---|---|
| unsloth 27B NVFP4 | 2,070,000 (est) | unsloth |
| nvidia 27B NVFP4 (reference) | 1,505,565 | NVIDIA |
| sakamakismile 27B Text NVFP4-MTP | 336,558 | sakamakismile |
| michaelw9999 27B NVFP4-MTP (daily driver) | 88,404 | michaelw9999 |
| michaelw9999 Qwopus3.6-27B-v2-MTP-NVFP4 | 99,100 (est) | michaelw9999 |
| michaelw9999 Qwopus3.6-27B-Coder-MTP-NVFP4 | 85,600 (est) | michaelw9999 |
| Brian6145 27B Opus+Sonnet NVFP4-MTP | 54,500 (est) | Brian6145 |
| llmfan46 27B heretic-v2 NVFP4 MTP | 11,869 | llmfan46 |
| utautako 27B NVIDIA-NVFP4-MTP | 15,500 (est) | utautako |

**Operator's take (michaelw9999):** daily-drives michaelw9999/Qwen3.6-27B-NVFP4-MTP-GGUF via llama.cpp MTP (--spec-draft-n-max 2). The 27B dense at NVFP4 is largest dense model fitting single 24GB Blackwell with context headroom. The 35B MoE at NVFP4 is largest MoE for same card. The 45-day community report (u/1uyukbe) and template-fix note (u/i_am_me0_0) are why this guide leans 27B-dense for agentic work and flags the deep-thinking loop as thing to disable first.

---

## KNOWN ISSUES

### 🔴 The "Deep Thinking" Tool-Loop Bug

Qwen3.6's native "deep thinking" mode causes agent tool-call loops: model emits reasoning monologue before tool JSON, and trace isn't preserved across turns.

**Fixes (pick one):**
1. Set `enable_thinking=false` AND `preserve_thinking=false` to fully disable
2. Set `preserve_thinking=true` to keep thinking stable across turns without the loop
3. Set reasoning budget to max 4096 tokens (community-tested, u/vexatious-big r/LocalLLM): "A reasoning budget of max 4096 tok is more than enough. Prevents 'but wait... actually...' loops."

**Best fix (community-reported):** Use froggeric patched chat templates:
> "One thing I discovered later is worse tool calling is because of bugs in standard chat template. On huggingface froggeric has patched templates. I use those and since that I have had almost no issues with tool calling for qwen 3.6 35b a3b q4km." (u/i_am_me0_0, +47, r/LocalLLM)
**Template repo:** https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates

**Reasoning-distilled variants** (lordx64, hesamation, Qwopus Coder, rico03, Brian6145) are less affected because they were trained on flattened reasoning traces.

**Loop risk severity by use case:**
- Agentic tool chains (Hermes workflows): HIGH loop risk with thinking ON — fix template first
- Chat / Q&A: Loop risk low if model is instruction-following fine-tune; use reasoning OFF entirely
- Long-horizon agent: Set reasoning budget 4096 max

### Temperature Fix for Repetition
Across Qwopus, HauhauCS, and lordx64, users report raising temperature to temp=1.0 fixes repetition loops. rico03 recommends temp=0.6 for reasoning models specifically.

### Long Context Beyond 32K
Use YaRN/RoPE scaling, NOT direct context window expansion:
```
--rope-scaling yarn --rope-scale 4 --yarn-orig-ctx 32768
```
Gives 128K context. Works identically for both 27B and 35B-A3B.

### IQ Quants vs K Quants
IQ (importance-aware) quants usually outperform same-size K quants. Prefer IQ3_M over Q3_K_M, IQ4_XS over Q4_XS, IQ4_S over Q4_S. DavidAU's per-quant benchmarks confirm this.

---

## QUICK PICK TABLE — By Situation

| Your situation | Download this | Why | Tradeoffs |
|---|---|---|---|
| Coding (27B, need raw SWE-bench) | **DavidAU Heretic NEO-CODE Q4_K_M** | Arc-c +4% vs base, uncensored | Not the best for agentic workflows that need reasoning |
| Coding (27B, want reasoning-enhanced) | **Jackrong Qwopus3.6-27B-Coder-MTP** | Reasoning traces improve structured coding | SWE-bench 67.0% = 10% BELOW base; only for structured workflows |
| Agents (35B) | **unsloth Qwen-AgentWorld-35B-A3B** | Fixes looping/tool-leakage; agent-tuned | Brand new (Jul 2026), independent benchmarks not yet published |
| Agents (27B) | **27B Q8 + MTP + reasoning OFF + froggeric template** | 95% agent success with tight schemas | Requires custom template + narrow schemas |
| Uncensored (either base) | **llmfan46 heretic (27B or 35B)** | KL 0.0021 = lowest drift from base; 6/100 refusals | No SWE-bench/AIME data published; capability preservation inferred |
| Reasoning (35B) | **lordx64 Opus 4.7 Distilled** | Opus reasoning style, dataset public, GSM8K 84.3%, MMLU-Pro 74.9% | Loop risk on chat; burns thinking tokens on simple questions |
| Reasoning (27B) | **rico03 Opus 4.6 Distilled** | Opus 4.6 style on community-preferred dense base | No SWE-bench/AIME delta vs base listed |
| Speed (Blackwell) | **NVFP4-MTP (michaelw9999 daily driver)** | ~100-120 t/s; largest dense/MoE fitting 24GB | 8-10% loss on SWE-bench, ~0.4 on AIME; Ampere/Ada won't load |
| Speed (non-Blackwell) | **unsloth MTP UD-Q4_K_M** | Zero quality loss at greedy, 1.5-2× speedup | Needs custom llama.cpp build with PR #22673 |
| Fastest MoE (Blackwell) | **michaelw9999 35B-A3B NVFP4-MTP** | ~100 t/s on MoE | Blackwell-only; NVFP4 tradeoffs apply |
| Math / AIME | **35B-A3B Q6_K_XL (Unsloth MTP)** | AIME 92.7-92.9% (base) | Slower than dense for same VRAM unless you offload |
| Structured output (JSON) | **DFlash on base model** | 2-3× speed on repetitive/structured work | Dies with offload; worse for creative text |
| Newcomer default | **DavidAU Heretic NEO-CODE (27B)** | Proven benchmark winner, uncensored, well-documented quants | Overkill if you don't need uncensored |
| Aggressive uncensored | **HauhauCS Uncensored-Aggressive** | 2M+ downloads = proven stable, 0/465 refusals | Provenance controversy (unverified); no benchmarks listed |
| Document analysis | **DavidAU 40B Opus-Deckard NEO-CODE** | Mixed community reports, +4-14% on some in-house benchmarks | Polarizing; no SWE-bench/AIME listed; some say "schizo models" |
| AMD 7900 XTX | **plunderstruck MTP-ROCmFP4** | ROCm FP4 path | Smaller community |
| Mac / low-VRAM | **Gemma 4 12B Q8 MTP** | Fast and reliable on Mac | Not Qwen — different ecosystem |
| Long-horizon agents | **Gemma 4 31B or 26B** | Some users prefer it over Qwen for long agent loops | Not Qwen — different ecosystem |

---

## GPU / VRAM MATRIX — File size + expected speed

| Card | VRAM | 27B Recommendation | 35B-A3B Recommendation |
|---|---|---|---|
| RTX 5090 / B200 | 24-80GB | NVFP4-MTP full (16.19 GB), ~100-120 t/s | NVFP4-MTP full, ~100 t/s |
| RTX 4080/4090 | 16-24GB | Q4_K_M MTP (~20 GB with context), ~60-80 t/s | Q4_K_M or Q6_K, ~18-20 t/s gen |
| RTX 3090 | 24GB | Q4_K_M (~19 GB with context), Q3_K_M (13.82 GB) tight context | Q4_K_M (22.66 GB) at modest context |
| AMD 7900 XTX | 24GB | plunderstruck ROCmFP4 or Q4_K_M via ROCm | Q4_K_M |
| Mac 24GB (M-series) | unified | Q4_K_M, watch unified memory + context | Q4_K_M |
| RTX 3060 12GB | 12GB | Q3_K_M (12.4 GB, ~15 GB with context) | Too small for quality |
| A100 / H100 | 40-80GB | Q8_0 at 128K context | Q5_K_M or Q6_K at 128K |

**Speed tips:**
- MTP adds ~1 GB overhead, ~1.5-2× speedup
- DFlash adds ~0.88 GB, 2-3× speed on structured work
- 35B-A3B MoE is faster than 27B dense on same hardware (only 3B active per token)
- Reasoning OFF for coding tasks: "Reasoning off is better for coding tasks. You use reasoning for research, log hunting, spec writing, but coding, just execute." (u/rchamp26)

---

## NOTABLE ABSENCES (as of July 20)

- **Jackrong Qwopus MTP for 35B-A3B**: Still not available. "Qwopus MTP: Requested, not delivered. Will be top-tier option when available." (May thread, still accurate July 20)
- **HauhauCS Balanced variant (27B)**: Never published. Only Aggressive exists for 27B. (35B has Balanced.)
- **rico03 35B variant**: Only 27B exists for rico03 Opus distilled. (Also has 35B opus reasoning GGUF but different model.)
- **mudler APEX 27B**: APEX is MoE-specific (for 35B). No 27B APEX exists.
- **27B Qwopus gap**: Partially closed by michaelw9999 Qwopus v2 NVFP4-MTP (88.4K DL) and Jackrong Coder-MTP line.
- **Qwen3.8**: Announced (top LocalLLaMA/localllm posts, July 19: "Prepare your VRAM — Qwen3.8 is coming"). Not yet released. This guide covers 3.6. Watch for 3.8 MoE and 3.8 dense.

---

## WATCH FOR

- ⚠️ NVFP4 needs Blackwell. On Ampere/Ada, NVFP4 files will not load — use FP8/FP16/GGUF instead.
- ⚠️ NVFP4 is still a quantization — expect 8-10% loss on SWE-bench / coding-heavy tasks. Not mathematically-lossless.
- MTP: no `-np > 1`, no `--mmproj` (vision) with MTP on some builds. Verify per backend.
- Deep-thinking loop: disable as described, OR use reasoning-distilled builds, OR set reasoning budget to 4096 tokens max.
- Download counts are root-repo aggregates and grow monotonically; treat any "decrease" as repo-ID mismatch.
- Froggeric chat template fixes tool-calling bugs. Use it if you're having tool-call issues: https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates

---

## EDITOR'S RIG (field-tested)

The operator runs michaelw9999/Qwen3.6-27B-NVFP4-MTP-GGUF on a Blackwell GPU via llama.cpp MTP (--spec-draft-n-max 2) as daily driver, spot-runs the 35B-A3B NVFP4-MTP. Both are NVFP4 + MTP. The 27B dense at NVFP4 is largest dense model fitting single 24GB Blackwell with context headroom; 35B MoE at NVFP4 is largest MoE fitting same card. The 45-day community report (u/1uyukbe) and template-fix note (u/i_am_me0_0) are why this guide leans 27B-dense for agentic work and flags deep-thinking loop as thing to disable first.

---

## SOURCES

**Community threads (verified July 19-20, 2026):**
- 85 GPU-hours comparing 5 abliteration methods on Qwen3.6-27B (nathandreamfast, r/LocalLLaMA): https://old.reddit.com/r/LocalLLaMA/comments/1tfmocw/
- Best Local Agents - Jun 2026 (r/LocalLLaMA, active through July): https://old.reddit.com/r/LocalLLaMA/comments/1uaebfe/
- Qwen 3.6 27B Makes Huge Gains in Agency (r/LocalLLaMA): https://old.reddit.com/r/LocalLLaMA/comments/1strodp/
- DavidAU/Qwen3.6-40B Discussion (r/LocalLLaMA): https://old.reddit.com/r/LocalLLaMA/comments/1tlykbg/
- lordx64 Opus 4.7 Distilled Release (r/LLMStudio): https://old.reddit.com/r/LLMStudio/comments/1spzwo8/
- I ran Qwen 3.6 locally for 45 days (u/1uyukbe, r/LocalLLM): https://old.reddit.com/r/LocalLLM/comments/1uyukbe/
- Best models for your hardware this week (r/LocalLLM): https://old.reddit.com/r/LocalLLM/comments/1u0w8g9/
- SWE-rebench leaderboard update: GLM-5.2, Qwen3.6-27B (r/LocalLLaMA): https://old.reddit.com/r/LocalLLaMA/comments/1uknx14/

**Model cards verified:**
- DavidAU: https://huggingface.co/DavidAU/Qwen3.6-27B-Heretic-Uncensored-FINETUNE-NEO-CODE-Di-IMatrix-MAX-GGUF (131K downloads, 402 likes) and https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF (16.7K downloads, 135 likes)
- Jackrong: https://huggingface.co/Jackrong/Qwopus3.6-27B-v2-MTP-GGUF (90.9K downloads) and https://huggingface.co/Jackrong/Qwopus3.6-27B-Coder-MTP-GGUF (125.3K downloads)
- lordx64: https://huggingface.co/lordx64/Qwen3.6-35B-A3B-Claude-4.7-Opus-Reasoning-Distilled (29.9K downloads, 197 likes)
- rico03: https://huggingface.co/rico03/Qwen3.6-27B-Claude-Opus-Reasoning-Distilled-GGUF (7.4K downloads)
- HauhauCS: https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive (2.0M downloads, 2,923 likes) and https://huggingface.co/HauhauCS/Qwen3.6-27B-Uncensored-HauhauCS-Aggressive (358K downloads, 527 likes)
- llmfan46: https://huggingface.co/llmfan46/Qwen3.6-27B-uncensored-heretic-v2-Native-MTP-Preserved-NVFP4-GGUF (KL 0.0021, MMLU 86.65% → 85.67%)
- nvidia: https://huggingface.co/nvidia/Qwen3.6-27B-NVFP4 (1.5M downloads) and https://huggingface.co/nvidia/Qwen3.6-35B-A3B-NVFP4 (8.5M downloads) — AIME 2025 data listed
- RedHatAI: https://huggingface.co/RedHatAI/Qwen3.6-35B-A3B-NVFP4 (1.93M downloads) — full NVFP4 benchmark table including SWE-bench 50.2% (NVFP4) vs 54.8% (BF16)
- Base models: https://huggingface.co/Qwen/Qwen3.6-35B-A3B · https://huggingface.co/Qwen/Qwen3.6-27B (official benchmarks)
- Froggeric patched templates: https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates

---

v4.5 — July 20, 2026: Major revision per Fable (Claude Fable 5) independent critique. Added:
- ⚠️ START HERE with "Don't pick if" column for each row
- 🔴 WHAT NOT TO PICK section (6 specific anti-recommendations)
- RedHatAI NVFP4 benchmark table proving 8-10% SWE-bench loss (was missing from v4)
- Qwopus Coder regression flag (67% vs base 77% — explicit now)
- llmfan46 27B with KL 0.0021 and MMLU 86.65% → 85.67% data (was incomplete in v4)
- HauhauCS "no benchmarks listed" disclaimer added to each 35B and 27B entry
- Quick Pick Table: Why column rewritten to be specific; added Tradeoffs column
- NVFP4 section now has RedHatAI benchmark table as evidence; "Honest take" framing
- Download counts refreshed from HF API (July 20 verification)
- lordx64 eval data added (GSM8K 84.3%, MMLU-Pro 74.9%)
- rico03 card benchmarks flagged as base model only, not distilled variant delta
- DavidAU 40B Fable-Fusion-711 added as separate variant with full benchmark table
