# Models, Providers & Plans Megathread — June 2026

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1ufrtsf/models_providers_plans_megathread_june_2026/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

**LAST UPDATED:** June 25, 2026
**Sourced from:** 31+ r/hermesagent threads, 290+ community comments, [May 2026 Models Megathread](https://www.reddit.com/r/hermesagent/comments/1tgbsuz/) by u/digitalnomadpdx

**Scope:** Cloud APIs, subscription plans, provider comparison, model selection, and routing strategies. For local/self-hosted models, see the [Mac/MLX Megathread](https://www.reddit.com/r/hermesagent/comments/1uc7rw5/) and [r/hermesagent Local Models Guide](https://www.reddit.com/r/hermesagent/comments/1stiwug/).

---

## TL;DR — What Should I Use?

| Decision | Community Pick | Runner-Up | Notes |
|----------|---------------|-----------|-------|
| **Best overall (paid API)** | DeepSeek v4 Pro (direct) | DeepSeek v4 Flash | Pro for orchestrator, Flash for workers/auxiliary. $60 got one user 8B tokens. |
| **Best value subscription** | OpenCode Go ($10/mo) | Minimax $10 token plan | OpenCode Go = "~$60 API credit for $10." Minimax = "virtually unlimited" background agent work. |
| **Best premium subscription** | Nous Portal ($20) | OpenAI Codex ($20 ChatGPT) | Fixed monthly cost, no billing surprises |
| **Best free model (no catch)** | owL-alpha (OpenRouter) | Nemotron 3 Super 120B (free) | "Absolute beast at tool usage." Best free tier for Hermes agentic tasks. |
| **Best coding model** | GPT-5.5 (via Codex) | DeepSeek v4 Pro | GPT-5.5 is the "undisputed king" for complex coding. |
| **Best orchestrator model** | GPT-5.4-mini | DeepSeek v4 Flash | Fast, cheap, handles 90% of routing/web search/light tasks. |
| **Best provider for predictable billing** | OpenCode Go + Minimax stack | Nous Portal | Subscriptions = no surprise $100 days. Go+Minimax = $20/mo for near-unlimited. |

---

## PART 1: Cloud Provider Comparison

### Tier 1 — Community Favorites

| Provider | Pricing | Model Access | Best For | Watch For |
|----------|---------|-------------|----------|-----------|
| **DeepSeek (direct API)** | Flash: ~$0.22/M in, $0.20/M out (cache reads $0.004/M). Pro: ~$0.44/M in, $0.87/M out. Real-world: $0.30–$1.30/day for Flash, $2–6/day for Pro. | v4 Pro, v4 Flash, Coder | Primary orchestrator, heavy coding, cost-sensitive workflows | Direct API 4-5x cheaper than via OpenRouter. Throttling/503 errors during US peak hours (single-digit t/s). |
| **OpenCode Go** | **$10/mo** ($5 first month). ~$60 worth of credits at list prices. | DeepSeek Flash/Pro, Minimax M3, MiMo 2.5 Pro, GLM, Kimi, and more | **Best overall value subscription.** Covers 90% of agent workload. No concurrency limits. | Lacks some multimodal models (Gemma 4). Weekly/monthly caps. |
| **Nous Portal** | $20/mo subscription | Hermes models, DeepSeek, Qwen, routing | All-in-one convenience, predictable billing | Some models cost extra credits; check usage dashboard. Using non-free models exhausts $20 quickly. |
| **OpenAI Codex** | $20/mo (ChatGPT Plus BYOK) | GPT-5.5 (undisputed king for coding), GPT-5.4-mini (ultra-fast orchestrator) | Complex coding, deep reasoning. Best used as "senior fixer" — not as daily driver. | Burns weekly rate limits fast if overused. OAuth only. Shares ChatGPT rate limits. |

### Tier 2 — Strong Alternatives

| Provider | Pricing | Model Access | Best For | Watch For |
|----------|---------|-------------|----------|-----------|
| **MiniMax** | **Historical June 2026 snapshot:** $10/mo token plan and the listed quotas. Commenters later reported that the $10 plan was replaced by a $20 plan; verify the current official plan before purchase. | M3, M2.7 | Background agent work and auxiliary tasks | Community quality descriptions are subjective. Verify current quotas, model names, and loop behavior. |
| **Xiaomi MiMo** | **Historical June 2026 snapshot:** $6/mo direct token plan; annual/token figures were community-reported. | MiMo 2.5, MiMo 2.5 Pro | Agentic tasks, vision, coding | Cache behavior appears route-specific: direct API reports differed from OpenRouter. Test cache hits and billing on the route you will use. |
| **Kimi/Moonshot** | Pay-per-token via OpenRouter or direct | K2.6 (very intelligent, strong tool calling), K2.7 | Best open-source Hermes main model. "Built my entire Debian server stack." | Strict quota limits. Occasional Chinese chars in output. Tends to overthink on coding. |
| **OpenRouter** | Pay-per-token (variable). Free tier available with $10 credit. | 200+ models. **owL-alpha (free)** is the best free model — "absolute beast at tool usage and coding." | Model experimentation, fallback chains, free-tier models | Variable pricing; same model costs 4-5x more through resellers. Avoid auto-routing. Pin models to specific providers. |
| **Gemini (Google)** | Pay-per-token / free tier | Flash 2.5 (free), Pro 2.5 | Free tier for light tasks, strong vision | Rate limits on free tier; OAuth subscription risky as BYOK |
| **Ollama Cloud** | $20/mo ($22 credits), $100 tier | Free/open models only | Hassle-free hosted local-style models | **3 concurrent connection limit** — cron jobs crash if chatting simultaneously. Recently degraded (server busy, slow tokens). No frontier models. |

### Tier 3 — Budget / Niche

| Provider | Pricing | Best For | Watch For |
|----------|---------|----------|-----------|
| **GLM 5.1 / 5.2 (NeuralWatt)** | Free $5 credit, then PAYG. GLM-5.1 is very efficient and cheap. | Deep reasoning when speed doesn't matter. Stable, reliable. | 5.2 pricier. Painfully slow (18hrs for what GPT-5.5 does in 1hr). Prone to looping. |
| **Anthropic Claude (sub)** | $20/mo subscription | Opus 4.7 — high-quality reasoning, code review | Agentic use explicitly discouraged by Anthropic. Too expensive as primary. Token hog. |
| **NVIDIA NIM** | Free tier | Nemotron 3 Super 120B (free) — best emergency fallback when GPT limits hit | Smaller ecosystem; genuinely free |
| **Grok / superGrok** | $10/mo or $30/mo X Premium | Multi-modality, voice, good tool calling | X Premium $30/mo gives only ~2hrs agent work. API gives much more. Weak at coding. |
| **NanoGPT** | $12/mo | Only if you need uncensored models | "Sketchy AF." Slow, low limits. Models overly verbose. Not recommended as primary. |
| **Qwen OAuth** | Subscription / pay-per-token | Qwen models direct | Newer provider; fewer community data points |
| **OpenCode Zen** | $10/mo | Curated model selection | Smaller model selection than Go. Go is the better value. |

---

## PART 2: Model Comparison — Which Model for Which Task

Community consensus on which cloud models excel at which role. All accessible via the providers in Part 1.

### 🥇 Tier 1 — Best-in-Class

| Model | Best For | Access Via | Real-World Notes |
|-------|----------|-----------|-----------------|
| **GPT-5.5** | Complex coding, deep reasoning, research | OpenAI Codex ($20/mo), OpenAI API | "Undisputed king" — from 6B-token test. Writes economic journal articles. Burns rate limits fast. |
| **DeepSeek v4 Pro** | Daily driver, heavy coding, multi-step synthesis | DeepSeek direct API, OpenCode Go, Nous Portal | "Smarter than Claude Sonnet." $60 for 8B tokens over 2-3 weeks. Best value powerhouse. |
| **DeepSeek v4 Flash** | Orchestrator, routing, 90% of daily tasks | DeepSeek direct API (free tier available), OpenCode Go | $0.30–$1.30/day. With `reasoning=xhigh` can exceed Pro in some domains. Cache hits at $0.004/M. |
| **Claude Opus 4.7** | Highest-quality reasoning, code review | Anthropic subscription ($20/mo), OpenRouter | Brilliant but expensive. Token hog. Not for daily driving. Best as CLI-invoked code reviewer. |

### 🥈 Tier 2 — Strong Performers

| Model | Best For | Access Via | Real-World Notes |
|-------|----------|-----------|-----------------|
| **GPT-5.4-mini** | Ultra-fast orchestrator, routing | OpenAI Codex, OpenAI API | Handles 90% of routing/web search. Pair with GPT-5.5 for heavy lifting. |
| **Kimi K2.6** | General daily agent, coding, tool calling | OpenRouter, Kimi direct API | "Built my entire Debian server stack and moved 5 agents." Very intelligent, inexpensive. Strict quotas. |
| **Minimax M3** | Background agent work, stable everyday use | Minimax $10 token plan, OpenCode Go | "As good as GPT-5.5-low." The $10/mo "virtually unlimited" plan eliminates token anxiety. |
| **MiMo 2.5 Pro** | Agentic intelligence, coding, vision | Xiaomi $6-13/mo plan, OpenCode Go | Strong multi-modality, good coding. "Steal at current price." No caching — burns faster. |
| **Gemini 3.1 Pro** | Research, multi-modal, strong in custom pipelines | Google API, OpenRouter | Mixed community reception but holds up in custom agent pipelines. |

### 🥉 Tier 3 — Budget & Specialist

| Model | Best For | Access Via | Real-World Notes |
|-------|----------|-----------|-----------------|
| **owL-alpha** | Best free model — tool usage and coding | OpenRouter (free tier) | "Absolute beast at tool usage." Rate-limited for heavy multi-step loops. |
| **Nemotron 3 Super 120B** | Emergency fallback, free coding | NVIDIA NIM (free), OpenRouter (free) | Best fallback when everything else is rate-limited. Free. |
| **GLM-5.1** | Deep reasoning when speed doesn't matter | NeuralWatt (free $5 credit) | Stable, reliable. Painfully slow (18hr vs 1hr for GPT-5.5). |
| **Grok 4.3** | Multi-modality, voice, general agent | Grok API, X Premium | API gives much more than $30/mo X Premium sub (only ~2hrs agent work). |
| **Gemini 2.5 Flash** | Free tier, strong vision, light tasks | Google API (free tier) | Free. Good for light automation. Rate limits on heavier use. |
| **Qwen 3.6** (API) | Reliable function calling, cheap API rates | OpenRouter, Qwen OAuth | Reliable tool use. Rate limits on free tier. |

---

## PART 3: Subscription vs Pay-Per-Use

### Use a subscription ($10-20/mo fixed) if:

- You want predictable billing (no $100 surprise days)
- You use Hermes daily for extended sessions
- You prefer "set and forget" without monitoring token burn
- You're new to Hermes and don't know your usage patterns yet

### Use pay-per-token (DeepSeek direct, OpenRouter) if:

- Your usage is bursty (heavy days, then light days)
- You're extremely cost-sensitive and willing to monitor usage
- You run multiple worker profiles on cheaper models
- You can self-manage fallback chains and routing

### Hybrid strategy (most common in community):

> *"I use OpenAI $20/mo + DeepSeek v4 Flash for workers. Pro for main orchestrator. Multiple providers so I never hit a single rate limit."* — from "Affordable and good Models" thread

Pattern: Subscription for primary → cheap pay-per-token for workers/auxiliary. This is the most-recommended approach across 5+ threads.

---

## PART 4: Provider Reliability & Gotchas

### DeepSeek
- ✅ Extremely cheap direct API ($0.22/M input Flash, $0.44/M Pro; cache hits at $0.004/M)
- ✅ v4 Pro widely considered better than Claude Opus 4.7 for agentic tasks. "Smarter than Claude Sonnet" — from 6B-token test.
- ✅ Direct API includes prompt caching (80-90% cache hit rate with repeated tool schemas — absurdly cheap)
- ✅ Real-world costs: $0.30–$1.30/day for Flash, $2–6/day for Pro, $60 for 8B tokens over 2-3 weeks (u/drwebb)
- ❌ OpenRouter markup: same model costs 4-5x more through OR resellers
- ❌ Pricing changes: v4 pricing restructured May 2026; monitor for future changes
- ❌ China-based; data sovereignty concerns for some users
- ❌ Throttling/503 errors during US peak hours (single-digit t/s). Community workaround: fallback chain to Flash or another provider.
- 💡 **Tip:** Use direct API, not OpenRouter. Pin to `api.deepseek.com`. Cache system prompt + tool schemas for massive savings.

### OpenRouter
- ✅ Access to 200+ models from one account
- ✅ Built-in fallback chains
- ✅ owL-alpha on free tier — "absolute beast at tool usage"
- ❌ Variable per-provider pricing — same model at different price points
- ❌ Community warns: "avoid silent auto-routing for anything that mutates state"
- ❌ Default routing can silently switch models mid-task
- 💡 **Tip:** Pin specific model IDs and set limits. Use as fallback pool + free tier only.

### Nous Portal
- ✅ $20/mo covers multiple models
- ✅ First-party integration with Hermes (Nous builds Hermes)
- ❌ Some models consume extra credits beyond subscription
- ❌ "Hermes Plus" Opus routing doesn't feel identical to native Claude Code
- 💡 **Tip:** Check usage dashboard regularly; some users report opaque credit consumption

### OpenAI Codex
- ✅ GPT-5.5 — "undisputed king" for complex coding
- ✅ OAuth auth — no API key to leak
- ❌ Burns weekly rate limits fast if used as primary
- ❌ Cannot use as pure API key provider; must use OAuth flow
- 💡 **Tip:** Use as "senior fixer" for hard problems only. Pair with a cheap orchestrator for daily driving.

### Minimax
- ✅ $10/mo token plan is "virtually unlimited" — 15K req/week
- ✅ M3 considered "as good as GPT-5.5-low"
- ❌ M2.7 reliable but uncreative. Prone to looping without guardrails.
- 💡 **Tip:** Best for background workers. Never worry about token burn on routine tasks.

### OpenCode Go
- ✅ $10/mo is the single best value subscription. "~$60 API credit for $10 — 6x value."
- ✅ Access to DeepSeek Flash/Pro, Minimax M3, MiMo 2.5 Pro, GLM, Kimi
- ❌ Lacks some multimodal models (Gemma 4). Weekly/monthly caps.
- 💡 **Tip:** Pair with Minimax $10 token plan for a near-unlimited $20/mo stack.

### Anthropic Claude (subscription)
- ✅ Opus 4.7 excellent for complex reasoning
- ❌ Subscription path only for Hermes — API key path explicitly forbidden
- ❌ Community reports of sudden denials for agentic use
- 💡 **Tip:** If you use Claude, route through OpenRouter instead of direct subscription

### Ollama Cloud
- ✅ Simple hosted inference for open models
- ❌ **3 concurrent connection limit** — cron jobs crash if chatting simultaneously
- ❌ Recently degraded (server busy, slow tokens). No transparency on capacity.
- 💡 **Tip:** Only use if you specifically need hosted open models and can tolerate the limits.

---

## PART 5: Model Routing & Fallback Chains

### Gold Standard: Two-Tier + Fallback Architecture

This is the pattern used by the most sophisticated Hermes setups (from the 6B-token test thread):

```
Tier 1 — Orchestrator (cheap + fast, handles 90%):
  GPT-5.4-mini, DeepSeek v4 Flash, or Kimi K2.6
  → General chat, routing, web search, lightweight tasks

Tier 2 — Powerhouse (expensive + smart, invoked only when needed):
  GPT-5.5 (via Codex), DeepSeek v4 Pro
  → Complex coding, deep research, multi-step synthesis

Fallback chain (when limits hit):
  GLM-5.1 → Nemotron 3 Super 120B (free) → owL-alpha
```

Community quote: *"Never use one model for everything. Smart routing buys you 5-10x more capability per dollar than any single subscription. The $10/mo OpenCode Go + proper routing beats any single $20/mo provider hands down."*

### Pattern 2: Multi-provider stack
```
Primary: DeepSeek v4 Pro (direct) or GPT-5.5 (Codex)
Orchestrator/Auxiliary: DeepSeek v4 Flash (direct) or Minimax M3
Workers: MiMo 2.5 Pro or Kimi K2.6 (OpenCode Go)
Fallback: owL-alpha or Nemotron (OpenRouter free tier)
```

### Pattern 3: OpenRouter as fallback pool
```
Hermes → pinned model IDs with fixed providers
Free tier: owL-alpha or Stepfun 3.7 Flash (pennies/day)
Never: OpenRouter → auto-routing → unknown model
```
Community warning: *"I would avoid silent auto-routing for anything that mutates state. The model can change mid-task and you won't know until it breaks."*

### Pattern 4: Profile-level routing (advanced)
```
Root profile: pure coordinator → routes to:
  coder profile → GPT-5.5 (Codex)
  researcher profile → Gemini 3.1 Pro
  pm profile → Minimax M3 or DS v4 Pro
```
From u/FinancialBandicoot75: "Now my profiles talk to each other."

### Pattern 5: Event-driven (lowest cost)
Poll cheaply with lightweight watchers. Only wake Hermes when a filter matches, instead of cron-based fixed schedules. Saves tokens on idle polling. Community project: Watchline by u/SinghCoder.

### Pattern 6: LiteLLM proxy (maximum resilience)
```
Hermes → LiteLLM → tiered provider pool
```
Several community members use LiteLLM for provider-level failover. More setup complexity but maximum resilience.

---

## PART 6: Cost Management

### What the community actually spends (verified from threads)

| Usage Level | Monthly Cost | Typical Setup | Real User Data |
|-------------|-------------|---------------|----------------|
| **Light** (occasional tasks) | $0-5 | OpenRouter free tier (owL-alpha) + DeepSeek Flash free | "0 cost, works for simple tasks" |
| **Budget** (daily, cost-conscious) | $10-15 | OpenCode Go ($10) + Minimax $10 token plan stack | "OpenCode Go = ~$60 credits for $10 — 6x value" |
| **Moderate** (daily assistant) | $15-30 | DeepSeek Flash daily + Pro for complex tasks | $0.30–$1.30/day Flash; $2-6/day Pro |
| **Heavy** (all-day coding agent) | $30-60 | DeepSeek v4 Pro direct API + Flash workers | $60 for 8B tokens over 2-3 weeks (u/drwebb) |
| **Power user** (multi-agent farm) | $60-200 | Multiple subscriptions + direct APIs | "Burning $4-6/day on DS v4 Pro" (u/Blaze6181) |

### Cost horror stories (learn from these)

- *"Burned through $10 in an hour"* — Claude Sonnet via OpenRouter at reseller markup. Switched to DeepSeek Flash, fixed.
- *"$100 per day for basic queries with Sonnet"* — switched to DeepSeek v4 Flash, problem gone.
- *"50K+ tokens on every single prompt"* — context bloat from skills/tools; trimming saved 60%.
- *"Burned through $10 in an hour"* (OpenRouter variant) — DeepSeek via OR at 4-5x markup. Switch to direct API.
- *"Ollama Cloud 3-connection limit killed my cron jobs"* — background tasks crashed when chatting simultaneously.

### Cost-saving tips (community consensus, ranked by impact)

1. **Use DeepSeek direct, not via OpenRouter.** 4-5x cheaper for the same model. Cache hits at $0.004/M.
2. **Two-tier routing: cheap orchestrator + expensive powerhouse.** Flash for 90% of tasks, Pro/Codex only for complex work. Saves 5-10x.
3. **Offload auxiliary tasks to Flash or Minimax.** Compression, title generation, session search don't need Pro.
4. **Trim your skills and toolsets.** Every enabled tool adds schemas to every prompt.
5. **Delegate heavy work to subagents on cheaper models.** See delegation patterns.
6. **Use prompt caching.** DeepSeek direct API caches repeated tool schemas automatically — 80-90% cache hit rate.
7. **Event-driven over cron polling.** Lightweight watcher → wake Hermes only when needed instead of fixed-schedule burns.
8. **Minimax $10 token plan for background workers.** "Virtually unlimited" at 15K req/week. Never worry about token burn on routine tasks.
9. **Set spending caps.** Every provider supports them. The $100 surprise day is preventable.
10. **OpenCode Go as primary + pay-per-token fallbacks.** Subscription covers steady usage; API keys cover overflow.

---

## PART 7: FAQ

**Q: What's the absolute cheapest way to run Hermes?**
A: OpenRouter free tier (owL-alpha — "absolute beast at tool usage") + DeepSeek v4 Flash free tier. $0/month. Expect rate limits. Next step up: DeepSeek v4 Flash direct at $0.30–$1.30/day.

**Q: What's the single best value setup for $20/month or less?**
A: OpenCode Go ($10) + Minimax $10 token plan. You get ~$60 in API credits from Go + "virtually unlimited" agent work from Minimax. Covers 90% of workloads. The most-recommended budget stack across all threads.

**Q: Should I use OpenRouter or direct APIs?**
A: Direct APIs are cheaper and predictable. OpenRouter adds 4-5x markup on DeepSeek. Use OR only for model experimentation, free tier models (owL-alpha), or as a fallback pool. Never as primary with auto-routing enabled.

**Q: Can I use my ChatGPT/Claude subscription with Hermes?**
A: ChatGPT Plus → yes, via OpenAI Codex OAuth (GPT-5.5 access). Claude subscription → technically possible but Anthropic explicitly disallows agentic use without API billing. Community consensus: don't risk your Claude account. Use OpenRouter for Claude access instead.

**Q: DeepSeek v4 Pro vs Flash — which for what?**
A: Pro for main orchestrator (complex reasoning, coding). Flash for workers, auxiliary tasks, title generation, compression. Flash is cheaper and faster; Pro is smarter.

**Q: Is Nous Portal worth $20/mo?**
A: Yes if you want one bill and multiple models. No if you're comfortable managing direct API keys and want the absolute lowest cost (DeepSeek direct is cheaper).

**Q: How do I avoid burning $50 in a day?**
A: Set spending caps in your provider dashboard. Use subscription plans for predictable billing. Never leave OpenRouter on auto-routing with expensive models.

**Q: What's the minimum context window for Hermes?**
A: 64K tokens minimum. Hermes injects tool definitions, skills, memory, and chat history. Below 64K you'll hit context overflow on the first complex task.

**Q: Which model should I use for coding vs general tasks?**
A: Coding: GPT-5.5 (Codex) or DeepSeek v4 Pro. General tasks/routing: GPT-5.4-mini, DeepSeek v4 Flash, or Kimi K2.6. Use a two-tier setup so you're not burning expensive tokens on simple routing.

**Q: How do I set up fallback chains?**
A: `hermes config` → fallback_providers, or use credential pools. For advanced routing, community members use LiteLLM as a proxy. See Part 5.

**Q: Which providers support prompt caching?**
A: DeepSeek (direct API), Anthropic (API), and OpenAI (API). Prompt caching saves 50-90% on repeated tool schema tokens. OpenRouter caching behavior varies by underlying provider.

**Q: OpenCode Go vs Zen — which should I pick?**
A: Go. Larger model selection, same price. Zen is simpler but gives you fewer models. Community consensus is unanimous: Go is the better deal.

**Q: What model for background/cron agents?**
A: Minimax M3 ($10/mo token plan — "virtually unlimited") or DeepSeek v4 Flash via OpenCode Go. Never use expensive models for background tasks.

---

## PART 8: Knowledge Table — Every Provider & Plan Mentioned

| Provider | Price (Monthly) | Free Tier | Key Models | Hermes Setup | Watch For |
|----------|----------------|-----------|-----------|-------------|-----------|
| DeepSeek (direct) | Pay-per-token | Flash free tier | v4 Pro, Flash, Coder | `hermes model` → DeepSeek | Direct API 4-5x cheaper than OR. US peak throttling. |
| OpenCode Go | **$10** ($5 first) | No | DeepSeek Flash/Pro, Minimax M3, MiMo 2.5 Pro, GLM, Kimi | `hermes model` → OpenCode Go | **Best value subscription.** Weekly/monthly caps. $60 credit cap. |
| Minimax | **$10** token plan | No | M3, M2.7 | `MINIMAX_API_KEY` in .env | "Virtually unlimited." 15K req/week. M2.7 uncreative; M3 better. |
| Nous Portal | $20 | No | Hermes + DeepSeek + Qwen | `hermes auth add nous` | Non-free models exhaust $20 quickly. |
| OpenAI Codex | $20 | No | GPT-5.5, GPT-5.4-mini | `hermes auth add openai-codex` | OAuth only. Burns rate limits fast. Use as "senior fixer." |
| Xiaomi MiMo | $6-13 | No | MiMo 2.5, MiMo 2.5 Pro | `XIAOMI_API_KEY` in .env | No caching. Annual plan ~$13/mo for 2.4B tokens. |
| Kimi/Moonshot | Pay-per-token | No | K2.6, K2.7 | `KIMI_API_KEY` in .env | Best open Hermes main model. Strict quotas. |
| OpenRouter | Pay-per-token | Free tier ($10 credit) | 200+ models inc. owL-alpha (free) | `OPENROUTER_API_KEY` in .env | 4-5x markup on DS. Pin models. Avoid auto-routing. |
| Anthropic (sub) | $20 | No | Opus 4.7, Sonnet 4.5 | OAuth via Hermes | Agentic use discouraged. Token hog. |
| Gemini (Google) | Pay-per-token | Flash free tier | Flash 2.5, Pro 2.5 | `GOOGLE_API_KEY` in .env | OAuth sub risky as BYOK. |
| GLM / NeuralWatt | Free $5 credit, then PAYG | Yes ($5) | GLM-5.1, GLM-5.2 | Custom endpoint | 5.1 efficient. 5.2 pricier. Painfully slow. |
| Ollama Cloud | $20-100 | No | Free/open models only | Custom endpoint | 3-connection limit! Degraded recently. |
| NVIDIA NIM | Free | Yes | Nemotron 3 Super 120B | Custom endpoint | Best emergency fallback. Free. |
| Grok / superGrok | $10-30 | No | Grok 4.3 | API key or X Premium sub | X Premium gives ~2hrs agent work. API much better. |
| NanoGPT | $12 | No | Various | — | Not recommended. Slow, low limits. |
| Qwen OAuth | Subscription | No | Qwen 3.6, Qwen 3.5 | `hermes auth add qwen-oauth` | Newer; fewer data points. |
| Stepfun AI | Free voucher ($100) | Yes | Stepfun 3.7 Flash | Custom endpoint | Voucher may no longer be offered. |
| GitHub Copilot | $10/mo | No | GPT-5.4 via Copilot | ACP transport | Separate from ChatGPT Plus. |
| OpenCode Zen | $10/mo | No | Curated models | `hermes model` → OpenCode Zen | Smaller selection than Go. |
| Dappnode Nexus | €20/mo (~$22) | No | Kimi K2.6, MiniMax 2.7, DS 3.2, GLM 5, Qwen | Custom endpoint | Private/anonymous models. Good for privacy-conscious. |

---

## Contribute

This megathread is community-maintained. If you spot an error, have pricing updates, or want to add provider data:

- **Comment below** with corrections — include source (your own usage, provider page, etc.)
- **Corrections welcome, debate welcome.** This is a snapshot of community consensus, not official advice.

**Local/self-hosted models:** See the [Mac/MLX Megathread](https://www.reddit.com/r/hermesagent/comments/1uc7rw5/) and [Local Models Guide](https://www.reddit.com/r/hermesagent/comments/1stiwug/).

---
*Compiled from 31+ r/hermesagent threads. All prices USD. Last updated June 25, 2026.*

---

## Comment-Sourced Updates

- **MiniMax pricing drift:** multiple commenters reported that the former $10 plan had been replaced by a $20 plan. Prices in the body are historical snapshots and must be checked against the provider before purchase.

- **Caching is route-specific:** commenters reported different cache behavior through direct MiMo access versus OpenRouter. Do not infer direct-provider behavior from one aggregator route.

- **Rankings are community reports:** claims that one premium subscription or model is categorically best remain opinion unless supported by reproducible task-specific evidence.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
