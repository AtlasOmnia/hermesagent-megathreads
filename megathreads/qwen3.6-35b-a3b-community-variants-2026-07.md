# Qwen3.6-35B-A3B Community Variants — Historical Guide for Limited Local Hardware

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/
>
> **Repository edition:** July 16, 2026
> **Original post snapshot:** May 24, 2026

This guide preserves the useful selection framework from the original post without presenting its historical download counts, benchmark scores, throughput figures, or quality rankings as current facts.

## Verification boundary

Before downloading a variant, verify all of the following on the current model card and runtime release:

- repository owner, model lineage, and license;
- exact quant filename and file size;
- backend compatibility and required build;
- vision, thinking, tool-calling, and MTP support;
- context length and KV-cache memory at the context you intend to use;
- benchmark methodology, hardware, flags, and capture date.

The original Reddit post contains detailed historical figures. Those remain available through the source link, while comment claims are classified in the companion provenance file.

## Base model

The variant family discussed here is built around `Qwen/Qwen3.6-35B-A3B`.

Official model card to check first:

- https://huggingface.co/Qwen/Qwen3.6-35B-A3B

The source post described it as a mixture-of-experts model with multimodal support and a large native context window. Use the official model card—not the historical Reddit figures—for current architecture and benchmark details.

## What community variants change

### Safety-removal and abliteration

These variants attempt to reduce refusals through weight-space edits or related techniques. “Uncensored,” “lossless,” and refusal-count claims are publisher/community descriptions, not guarantees of unchanged capability.

Community repositories mentioned in the source post include:

- `HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive`
- `LuffyTheFox/Qwen3.6-35B-A3B-Uncensored-Wasserstein-GGUF`
- `llmfan46/Qwen3.6-35B-A3B-uncensored-heretic-GGUF`
- `huihui-ai/Huihui-Qwen3.6-35B-A3B-abliterated`
- mradermacher Abliterix/EGA conversions

Treat each as a separate derivative. Review its license, conversion notes, evaluation method, and upstream lineage before use.

### Reasoning-distilled variants

These variants were described as fine-tuned on reasoning traces from larger models. Distillation can change reasoning style, verbosity, repetition, coding behavior, vision quality, and tool-call reliability.

Repositories mentioned in the source post include:

- `Jackrong/Qwopus3.6-35B-A3B-v1-GGUF`
- `lordx64/Qwen3.6-35B-A3B-Claude-4.7-Opus-Reasoning-Distilled`
- `hesamation/Qwen3.6-35B-A3B-Claude-4.6-Opus-Reasoning-Distilled-GGUF`
- `huihui-ai/Huihui-Qwen3.6-35B-A3B-Claude-4.7-Opus-abliterated`

The original post’s comparative quality and speed statements were not independently reproduced during migration. Test candidates with your own Hermes tasks and tool schemas.

### APEX conversions

The source post described APEX as a mixture-of-experts-oriented quantization approach rather than a new base model. It mentioned base, reasoning-distilled, and MTP-enabled APEX conversions from the LocalAI/mudler ecosystem.

Examples named in the source post:

- `mudler/Qwen3.6-35B-A3B-APEX-GGUF`
- `mudler/Qwen3.6-35B-A3B-APEX-MTP-GGUF`
- reasoning-distilled APEX/MTP conversions
- `mudler/Carnice-Qwen3.6-MoE-35B-A3B-APEX-MTP-GGUF`

Verify that the current runtime supports the exact quantization format before downloading.

### MTP and speculative decoding

Multi-Token Prediction support changed while the Reddit discussion was active. The original post referred to a custom llama.cpp build; commenters later reported support reaching a newer llama.cpp branch.

Current rule:

1. Check the current llama.cpp release notes or branch documentation.
2. Confirm that the exact model contains a compatible MTP head.
3. Confirm whether vision, parallel decoding, and your chosen cache format work with that build.
4. Benchmark with and without MTP on the same prompt set and context.
5. Do not assume a speedup or unchanged quality from another user’s hardware.

MTP-related repositories mentioned in the source post include Unsloth, havenoammo, byteshape, llmfan46, huihui, and mudler conversions.

DFlash was described as a separate draft-model approach rather than a standalone chat model. Verify current runtime support and training status before considering it.

## Hardware selection framework

Do not select a model by GPU VRAM alone. Estimate the complete runtime footprint:

- quantized model file;
- KV cache at the intended context;
- vision projector, if used;
- MTP or other draft head;
- runtime and CUDA/Metal/ROCm overhead;
- concurrent slots;
- CPU offload and system RAM headroom.

### Very constrained systems

Low-bit quants may fit, but capability and tool-call reliability can degrade sharply. Keep context modest, test the real Hermes tool loop, and compare against a remote API before committing to the local route.

### Mid-range systems

Choose a quant that leaves headroom for cache and runtime overhead instead of selecting the largest file that barely loads. Mixture-of-experts CPU offload may help, but it changes latency and throughput.

### Higher-memory systems

Use the additional headroom for context, cache precision, or a less aggressive quant. Larger context still requires explicit memory planning; the model file fitting does not prove the agent workload will fit.

## Hermes-specific validation

Before making a variant your primary model, run a small repeatable suite:

1. list tools and call a harmless read-only tool;
2. perform a multi-step file task inside a sandbox repository;
3. test malformed-path and missing-file recovery;
4. test context compression and a long tool result;
5. verify structured output or tool-call JSON;
6. test vision separately if the variant claims multimodal support;
7. repeat the same suite with thinking enabled and disabled;
8. compare MTP on and off using identical prompts.

Record the model ID, quant, backend commit/release, flags, hardware, context, cache type, and date.

## Tool-loop troubleshooting lead

Commenters associated repeated tool calls with `Preserve_thinking` and `Enable_thinking` behavior on some model-card/runtime combinations. That is a troubleshooting lead, not a universal fix.

If loops occur:

- reproduce with one simple tool;
- inspect the raw tool-call payload;
- test thinking on and off;
- reduce ambiguous instructions;
- verify timeout/retry behavior;
- compare a different quant or base variant;
- confirm the same task works on a known-good model.

## Publisher controversy

The comment thread contains accusations and counterclaims concerning one variant publisher. This repository does not adjudicate them. The provenance notes retain neutral summaries and source links. Anyone proposing a warning or removal should provide primary repository history, license text, commits, and attributable evidence.

## Comment-Sourced Updates

- **MTP status changed during publication:** do not follow the original custom-build statement without checking the current runtime.
- **Thinking/tool loops:** thinking-related settings were reported as a possible contributor on some builds; reproduce before changing defaults.
- **Performance claims:** speedups, timeout values, context recommendations, and benchmark tables are dated community reports.
- **Quant guidance:** verify the exact filename on the current model repository; similarly named IQ/K quants are not interchangeable.
- **Reputational claims:** allegations and defenses remain unverified and are excluded from canonical recommendations.

## Maintaining this guide

Open an issue or pull request with:

- exact model-card URL;
- lineage and license;
- quant filename;
- backend and version/commit;
- hardware, context, cache, and flags;
- reproducible prompt or benchmark method;
- date tested.

No referral links, affiliate links, unsupported promotional claims, or unsourced allegations.
