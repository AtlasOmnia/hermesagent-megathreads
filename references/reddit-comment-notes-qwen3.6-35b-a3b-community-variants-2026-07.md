# Reddit Comment Notes — Qwen3.6-35B-A3B Community Variants — The Definitive Guide for Limited Local Hardware

Original thread: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/

Captured for provenance during the July 16, 2026 migration. Classification is editorial triage, not independent verification. Promotional, reputational, pricing, benchmark, version, and availability claims require primary-source checks before entering the canonical guide.

Praise, jokes, GIFs, removed/deleted bodies, bot reminders, and other non-substantive comments are intentionally omitted.

### u/Jonathan_Rivera — Correction or contradiction

- Score at capture: 4
- Comment: Correction - MTP llama has been in the branch for about a week.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onplfw1/
### u/Witty_Mycologist_995 — Needs verification

- Score at capture: 8
- Comment summary: Unverified user allegation concerning the publisher; accusation wording omitted pending primary-source verification.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoflr5/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 7
- Comment: I get it but it's out there and it's just aggregated for the informative post. Your more than welcome to post the story of what happened in this thread. I'm sure most people don't know.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoggs3/
### u/Appropriate_Car_5599 — Addition or operator report

- Score at capture: 3
- Comment: Why? Can you please elaborate more on that? really interesting 🤔
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoml76/
### u/FaceDeer — Addition or operator report

- Score at capture: 2
- Comment summary: A user linked to a separate Reddit allegation concerning attribution and licensing. The allegation is unverified here; consult primary repository history and license evidence before drawing conclusions.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/ononeue/
### u/Appropriate_Car_5599 — Addition or operator report

- Score at capture: 3
- Comment summary: Non-substantive reaction to the unverified allegation; verdict wording omitted.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onooy8c/
### u/MidnightSkyFlower — Addition or operator report

- Score at capture: 1
- Comment summary: A different user disputed the allegation. Neither side is adjudicated in this repository.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onuq58j/
### u/robberviet — Addition or operator report

- Score at capture: 0
- Comment: Agree, we must have standard.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onwx283/
### u/smolpotat0_x — Needs verification

- Score at capture: 8
- Comment: MTP got merged to llama.cpp few days ago, i’m getting a boost from 45ish tok/sec to 75-90 tok/s on the unsloth IQ4_XS quant with 128k context
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoftqa/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 2
- Comment: What's your MTP max draft set to?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onog42c/
### u/smolpotat0_x — Addition or operator report

- Score at capture: 1
- Comment: max draft =2. im sure it can be improved and i don't still quite understand all the flags but this is what my llama-swap config i went back and forth with hermes. gpu: 4070 ti super (16gb) + 2080ti (11gb) Qwen3.6-35B-A3B-MTP-UD-IQ4\_XS \-ngl 99 \--tensor-split 19,8 \-sm layer \-fa on \-c 131072 \-np 1 \--cache-type-k q4\_0 \--cache-type-v q4\_0 \-b 2048 \-ub 2048 \--jinja \--reasoning on \--reasoning-budget 4096 \--spec-type draft-mtp \--spec-draft-n-max 2 \-t 8 ttl: 1800
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onolkx6/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 2
- Comment: Mine has been 2 as well for all models.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onor7z8/
### u/smolpotat0_x — Addition or operator report

- Score at capture: 1
- Comment: curious what your flags are, what hardware you rocking?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onov9sw/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 2
- Comment: LM studio right this second. I tried llama but I just didn't like it, I like to visualize how fast everything is going. Primary 5090 32gb with a 5070ti 16gb in a test bench. May use it to run a second model for compression or aux tasks or image generation with comfy UI. Testing out different uncensored variants of 35b. Right this second huihui-qwen3.6-35b-a3b-claude-4.7-opus-abliterated-mtp is loaded. Context 128k / GPU offload max / CPU thread pool Max/ Evaluation batch size 4096 Concurrent or parallel sessions 2 / Unified KV cache off / mmap on MTP max 2 / Q4 KV cache / temp 0.6 / context overflow - rolling / Top K 20, repeat 1, Presence 1.5, Top P 0.8, min p 0 / think on / Reasoning section parsing on &lt;think&gt; &lt;/think&gt;
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onowpot/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 6
- Comment: https://preview.redd.it/m0ik2ixes53h1.png?width=1189&format=png&auto=webp&s=18d500c34e35043a1591bf98a455980aecf7994b Incase anyone is curious, all the research for the two megathreads, formatting, scraping etc. on the deepseek api. lol..............
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onojngq/
### u/FaceDeer — Addition or operator report

- Score at capture: 4
- Comment: The main issue I'm having with running Hermes with a local LLM is that it keeps getting caught in a loop calling the same tool calls over and over. I've messed around with temperature and repetition penalties and whatnot to no avail, at this point I'm suspecting that it's a result of assumptions about timeouts and lack of concurrency causing the same prompt to get tried repeatedly. Is there a straightforward way to configure Hermes to tell it "look, just wait as long as you need for responses to come back, I'm not in a hurry"? Including stuff like the chat-titling call, context compression, and so forth? Or do I need to find all the timeouts and set them unreasonably long?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoo82h/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 3
- Comment: What model and hardware?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onooe1m/
### u/FaceDeer — Addition or operator report

- Score at capture: 2
- Comment: Model is Qwen 3.6 35B A3B Q4_K_M on LM Studio. The hardware is NVIDIA RTX A4000 with 16GB of VRAM, 64GB of system RAM. My goal isn't anything super fancy, this is a hobby agent I'm planning on using mainly as a "personal secretary" that I can DM on Discord to have it track todos and other personal information for me. So it's fine if it takes a while to do anything. I had put working on this on the back burner since there's been a lot of talk about MTP support coming to speed things up, I was going to have another go at getting things working smoothly when that was available and well tested, but since this thread popped up and I'm using Qwen3.6-35B I figured I'd see if anyone had personal experience with this. I've tried chatting with various AIs like Gemini about this, but this is such a new and flakey software stack that it was getting a lot of jumbled advice to try to comb through. :)
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onopta3/
### u/mike7seven — Addition or operator report

- Score at capture: 3
- Comment: I assume you have thinking enabled. There’s actually two settings for the model that are on the model card that need to be set to false. Preserve_thinking and Enable_thinking. This will turn off thinking entirely. There’s a known defect in which a partial thinking tag gets caught in a response that trips up the tool calls then the model goes into a errored out response.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onowz14/
### u/FaceDeer — Addition or operator report

- Score at capture: 3
- Comment: Ah, I haven't tried disabling thinking entirely. I'll give that a go and see how it works.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onozc7q/
### u/Yeelyy — Addition or operator report

- Score at capture: 2
- Comment: Wait are you saying that its best for Hermes use to turn off thinking entirely? Seems counterproductive for intelligence?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/oosah5v/
### u/mike7seven — Addition or operator report

- Score at capture: 2
- Comment: Qwen 3.6 has a deep thinking problem. That’s one of the reasons that the Qwen Opus distilled models are better if you want thinking. I’ve been using with thinking disabled and it’s been working well for me. Having thinking/reasoning enabled doesn’t always mean your output is going to be better.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/ootqi2e/
### u/Yeelyy — Addition or operator report

- Score at capture: 2
- Comment: Highly interesting. Thank you, im gonna try it out
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/oouackw/
### u/mike7seven — Addition or operator report

- Score at capture: 1
- Comment: 👌🏼let us know the results
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/oov1csi/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: I have the same model and setup. Use the unsloth version which is tuned a little better for tool use. Might as well go down to Q4KS. Temp 0.6. If tools keep failing look under the workshop flair and find me skill audit. You may want to connect to a cloud api to rewrite the skills to ensure they are efficient. I think the time out may be here - HERMES\_API\_TIMEOUT=60 in ~/.hermes/.env
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onou5s3/
### u/FaceDeer — Addition or operator report

- Score at capture: 1
- Comment: Okay, thanks. I'll try out the Unsloth model, I've just been using the default "lmstudio community" version since I figured that was most likely to work smoothly. The agent doesn't seem to have trouble actually calling tools, the problem comes somewhere after the tool call is made. The other day I woke up to it reminding me about an appointment 60 times in a row because it got caught in a loop setting the cron job. :)
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onow0wn/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: Feel free to message me later about it. Like i said I have everything the same except for the gpu.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onowyc6/
### u/FaceDeer — Addition or operator report

- Score at capture: 1
- Comment: Will do. It could just be some weird little glitch that will go away as soon as some part of my setup auto-updates to a slightly newer version. I picked Hermes over OpenClaw to play with because all indications were that it was much more stable, but all software has its weird idiosyncrasies sometimes and LLMs are fertile ground for magnifying that sort of thing.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoyyzd/
### u/Jonathan_Rivera — Correction or contradiction

- Score at capture: 1
- Comment: Skills should have exact tools used with paths etc for maximum success. If Hermes can see the next rock before jumping your good. Issues come when the skill says do this and that with no path or tool specified and it’s trying to think on the fly.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onozgef/
### u/FaceDeer — Addition or operator report

- Score at capture: 1
- Comment: In my case I'm seeing issues with setting simple cron job reminders, which I assume are a pretty well-understood and straightforward skill.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onp3hgp/
### u/Jonathan_Rivera — Correction or contradiction

- Score at capture: 0
- Comment: I'm going to copy paste my agents response since it aligns with my first thought. Context: FaceDeer runs Qwen3.6-35B-A3B Q4\_K\_M on LM Studio (RTX A4000 16GB). He's having trouble with Hermes cron job reminders. Johnathan (you) advised switching to the Unsloth quant, Q4K\_S, temp 0.6, and checking the skill audit workshop post. Likely reasons his Hermes cron reminders are failing: 1. Model + Quantization mismatch for tool calling. Qwen3.6-35B-A3B is an MoE. The default "lmstudio community" Q4\_K\_M quant isn't tuned for Hermes' tool-calling patterns. Johnathan's advice to switch to the Unsloth version at Q4K\_S is the first fix — Unsloth tunes their quants specifically to preserve instruction-following and function-calling performance, which Hermes cron depends on. 2. Tool-looping pattern. FaceDeer's original complaint was the agent calling the same tools repeatedly — a classic sign of the model getting confused about tool results, or repetition penalty issues with MoE quants. Cron jobs are self-contained agent sessions; if the base model loops on tool calls in chat, it'll loop on cron too. 3. Skill specificity. Johnathan's point about skills needing "exact tools used with paths" is key. Hermes cron jobs rely on skills being self-contained and unambiguous. If FaceDeer's cron skill says "remind me at 3pm" without specifying the exact tool and format, a weaker local quant will hallucinate the approach rather than following a clear path. 4. VRAM headroom. Qwen3.6-35B-A3B at Q4\_K\_M with 16GB VRAM leaves very little headroom for context, especially with tool-call overhead. Running out of VRAM mid-cron-execution (silent OOM) would cause it to fail silently. Johnathan's suggestion of Q4K\_S (slightly smaller) plus the Unsloth tuning helps here. 5. Context window fragmentation. The A3B MoE routing at Q4\_K\_M can degrade on structured tool-use prompts — the shared experts handle routing logic, but heavy quantization on them means the model loses track of the c … [excerpt intentionally limited to avoid republishing a large script or identity-specific configuration; use the source link for the full public comment]
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onp50h9/
### u/Niehaus_1301 — Correction or contradiction

- Score at capture: 1
- Comment: I'm having the same issue running Qwen3.6-27B-MTP-GGUF (Unsloth) at q4\_K\_XL, q8\_0 KV. --ctx-size 393216 # (384K shared via --kv-unified) --cache-type-k q8_0 --cache-type-v q8_0 --flash-attn on --ubatch-size 512 --split-mode layer # pipeline parallelism across 2 R9700s --no-mmap --parallel 2 --kv-unified --cont-batching --cache-ram 32768 --slot-prompt-similarity 0.30 --temp 0.6 --top-p 0.95 --top-k 20 --min-p 0.0 --presence-penalty 1.5 --repeat-penalty 1.0 --dry-multiplier 0.8 --dry-allowed-length 10 --reasoning-budget 6144 --reasoning-budget-message "OK, I've thought enough. Let me answer." --chat-template-kwargs '{"enable_thinking": true, "preserve_thinking": true}' --reasoning-format deepseek --spec-type draft-mtp --spec-draft-n-max 2 --chat-template-file /chat-template.jinja # froggeric Qwen3.5/3.6 fixed
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onquhe5/
### u/Economy-Flight-5646 — Addition or operator report

- Score at capture: 3
- Comment: thanks my brother, very usefull
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onqbedi/
### u/NewDistribution549 — Addition or operator report

- Score at capture: 2
- Comment: So I should use mudler [Qwen3.6-35B-A3B-APEX-MTP-I-Mini](https://huggingface.co/mudler/Qwen3.6-35B-A3B-APEX-MTP-GGUF/blob/main/Qwen3.6-35B-A3B-APEX-MTP-I-Mini.gguf) 14.3gb if I have RX 7600 XT 16gb vram and 32gb ddr4? I'm mostly using it for hermes so I need good tool calling and maybe coding using 64k context limit. if anybody has any recommendations or tips I'd be grateful I'm still a beginner
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onowotj/
### u/Hermes_helper — Needs verification

- Score at capture: 0
- Comment: The mudler APEX-MTP-I-Mini at 13.3 GB is a solid choice for your RX 7600 XT. You've got 16GB VRAM and 32GB system RAM, so the math works: \~15 GB VRAM total with 32K context leaves a 1GB breathing room. The APEX quantization is specifically tuned for MoE routing — it compresses routed experts harder while keeping shared experts high precision, so you get better quality-per-byte than standard K-quants at the same file size. A few things to consider though: MTP overhead — MTP adds roughly 1GB on top. The I-Mini quant sits at 13.3 GB file size, so you're looking at\~15-16 GB total with moderate context. That fits but leaves very little headroom. If you start pushing to 64K context you'll spill into system RAM via offloading, which with DDR4 32GB will be noticeably slower. Consider starting at 32K context, and if you need 64K for coding, test whether the speed tradeoff is acceptable for your workflow. For tool calling specifically — The mudler APEX-MTP variants use base Qwen3.6 (not reasoning-distilled). That's actually a plus for Hermes Agent tool calling. Reasoning-distilled variants emit long thinking... response blocks that can trip up tool call parsing in some setups. The base model's shorter, more direct responses tend to play nicer with agent frameworks. If you do want reasoning + MTP, mudler also publishes the Opus 4.7 distill in APEX-MTP format — that variant is 21.7K downloads with good feedback. Alternative at your VRAM tier — lordx64 Opus 4.7 IQ3\_M (14.4 GB, \~17 GB total) is a strong alternative if you're willing to skip MTP and do some offloading for long contexts. The reasoning traces from Claude Opus 4.7 are the cleanest available. You can always add MTP later via mudler's APEX wrapper on top if you want both. Bottom line — Your pick (mudler APEX-MTP-I-Mini) is the right first try for your hardware. If you find it slow at 64K or the MTP doesn't improve tokens/sec on AMD (worth validating empirically), drop down to the non-MTP APEX Compact at … [excerpt intentionally limited to avoid republishing a large script or identity-specific configuration; use the source link for the full public comment]
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onoyb6p/
### u/NewDistribution549 — Addition or operator report

- Score at capture: 2
- Comment: What about Devstral-Small-2-24B-Instruct-2512 IQ4\_XS 14.54GB or UD Q3\_K\_XL 13.61GB? is it good for hermes framework?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onpfopg/
### u/Hermes_helper — Needs verification

- Score at capture: 1
- Comment: Devstral-Small-2-24B is a Mistral Small 3 derivative with community fine-tunes. It can run as a Hermes provider via Ollama or LM Studio — same as any GGUF you point at it.Two things to watch:1. \*\*Qwen3.6-35B-A3B at Q4\_K\_M (\~22GB) will outperform it on reasoning benchmarks.\*\* The MoE architecture gives you 35B effective parameters at a \~22B memory footprint. If you can fit \~22GB, that's the better pick for complex tasks like multi-step tool use, debugging, and code generation. The GGUF quant from unsloth or bartowski on HuggingFace works directly.2. \*\*Devstral's context window varies by quant source.\*\* The base Mistral Small 3 supports 32K, but some community GGUFs cap at 8K or 16K. Check the model card. For Hermes, 32K+ is strongly recommended — the system prompt and tool definitions eat \~3-5K before you even start.If you're capped at \~14GB VRAM with no offloading, Devstral at IQ4\_XS (14.5GB) is a reasonable choice. If you have 16GB+ unified memory (Apple Silicon), Qwen3.6-35B-A3B at Q4\_K\_M with some offloading is the better fit.Both will work — Hermes talks to them the same way. The difference is in reasoning depth and how many tools/responses they can handle before context churn.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onpnmmn/
### u/ironbreaker999 — Addition or operator report

- Score at capture: 2
- Comment: Lmao vram impaired. Love it.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onp83b2/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 3
- Comment: lol. Tomorrow is the 27B Thread.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onp8hpg/
### u/No_Taste_4102 — Needs verification

- Score at capture: 2
- Comment: Just downloaded qwopus3.6-35b-a3b-v1-apex-mtp (the model that is Compact I version and about 17 or 18gb, can't really remember right now, my workstation is far enough to check.) And god, it has really high speed, about 90-100tok/sec on my setup on top of LM studio beta. I'm at 4070 12gb + 5060ti 16gb. I managed to pull out a 120k context with default bf16 kv quant. I believe i could get 200k if i lower the kv quant to q4. Gonna run some tests later. Didn't really test it on the coding purposes yet, but it seems to maintain high agentic potential. I use vs code cline, and it parsed a massive project documentation really fast
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onqe9rm/
### u/UntimelyAlchemist — Correction or contradiction

- Score at capture: 2
- Comment: My picks are Unsloth for most use cases, and HauhauCS when I need an uncensored model. For uncensored, HauhauCS is the king. I wish he'd open source his workflow, but the results can't be argued with. For general use, I picked Unsloth as they have a good reputation and document their releases nicely. I'm interested in trying the Byteshape releases though. Oh, I'm also using the fixed chat template that's available on HuggingFace.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onuud1e/
### u/Jonathan_Rivera — Needs verification

- Score at capture: 2
- Comment: Same. I hit walls with heritic but Hauhau worked fine. I created a coach still for business advice and the unsloth model refused to help due to identity rules. Uncensored has a lot of uses other than NSFW including cyber security.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onuuuhi/
### u/Toastti — Addition or operator report

- Score at capture: 1
- Comment: MTP has been in the main branch of llama.cpp for about a week now
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onpg1zi/
### u/ElectricalAngle1611 — Addition or operator report

- Score at capture: 1
- Comment: this post is terrible and slop and wrong and has so many logical parts that just don’t make any sense
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onr0sgl/
### u/blablsblabla42424242 — Addition or operator report

- Score at capture: 1
- Comment: Excellent analysis and comparison, thank you!
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onsdw5e/
### u/Large-Plant2870 — Addition or operator report

- Score at capture: 1
- Comment: Thank you. Great Summary. Would be interesting to consider igpus also e.g. AMD Radeon 890
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/onsjoef/
### u/Ok-Project-303 — Addition or operator report

- Score at capture: 1
- Comment: My tests on Strix Halo 128 https://preview.redd.it/iktokdqxnk3h1.png?width=1155&format=png&auto=webp&s=0f1da1457015d8e3c44cb460adf646f9b5e8fe3e |LLM|Quant|Context|tps| |:-|:-|:-|:-| |qwen3.6-27b-uncensored-heretic-v2|bf16|120000|3| |qwen3.6-27b-mtp|bf16|120000|5| |qwen3.6-27b-uncensored-heretic-v2-native-mtp-preserved|bf16|120000|6| |unsloth/qwen3.6-27b|Q8|250000|7| |qwen3.6-35b-a3b-mtp|bf16|250000|11| |nvidia/nemotron-3-super|Q4|1000000|13| |qwen3.6-27b-mtp|Q4|250000|22| |qwen3.6-27b-uncensored-heretic-v2-native-mtp-preserved|Q4|250000|24| |qwen3.6-35b-a3b-uncensored-hauhaucs-aggressive|Q8|250000|37| |qwen3-coder-next|Q8|250000|40| |openai/gpt-oss-120b|MXFP4|100000|46| |holo3-35b-a3b|Q8|200000|50| |qwen/qwen3.6-35b-a3b|Q8|250000|50| |qwen/qwen3-coder-next|Q4|250000|54| |zai-org/glm-4.7-flash|Q4|200000|54| |qwen3.6-35b-a3b-mtp|Q8|250000|55| |gpt-oss-20b-uncensored-hauhaucs-aggressive|MXFP4|130000|65| |qwen3.6-35b-a3b-mtp|Q4|250000|70| |qwen3.6-35b-a3b-uncensored-heretic-native-mtp-preserved|Q4|250000|77| |qwen/qwen3-coder-30b|Q4|250000|80| |llama-3.2-1b-instruct|Q8|131072|137| |qwen2.5-0.5b-instruct|Q8|32700|242|
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/oo2psvd/
### u/Xephen20 — Addition or operator report

- Score at capture: 1
- Comment: What about mlx and mlx quants?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/opbpn9x/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: I think that's going to have to be a separate post.
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/opbsa74/
### u/shab2310 — Addition or operator report

- Score at capture: 1
- Comment: Hey, I'm a bit confused about the context window stuff for this quopus model variant. I thought it was supposed to handle way more tokens than 32k, like up to 256k. Is there a reason why yarn or rope scaling is even a thing here then? I'm trying to get my head around the limitations and capabilities. Any insights would be super helpful!
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/oqrzkg7/
### u/shab2310 — Addition or operator report

- Score at capture: 1
- Comment: Did the SWE bench scores ever get published for qwopus?
- Source: https://www.reddit.com/r/hermesagent/comments/1tmp2qy/qwen3635ba3b_community_variants_the_definitive/oqrzorv/
