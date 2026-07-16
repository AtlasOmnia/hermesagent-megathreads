# MEGATHREAD: Hermes Agent Use Cases — What the Community is Building (May 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1t6gf4j/megathread_hermes_agent_use_cases_what_the/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

Part 1 - Part two in a sticky post below.

A community-sourced compilation of real-world Hermes Agent use cases, pulled from Reddit, X/Twitter, GitHub (issues, PRs, community repos), YouTube, Hacker News, blogs, podcasts, LinkedIn, Product Hunt, and GitHub Gists.

**240 source rows retained across 11 categories in this repository snapshot.** The original Reddit post described a larger 276-use-case/16-category corpus; not all entries were present in the captured post body. This file reports only what is actually preserved below.

> **Source-label caveat:** Rows that say only `GitHub`, `GitHub Gist`, `X`, `Blog`, `Reddit`, `LinkedIn`, `Product Hunt`, `Hacker News`, or `Podcast` did not preserve a direct URL in the captured post. Treat those as unverified leads, not sourced claims.

---

## Dev Workflow (64 retained rows)

People using Hermes for software development, codebase analysis, CI/CD, multi-agent build pipelines, and developer tooling.

| Use Case | Source |
|----------|--------|
| 12 Hermes instances every day, in parallel — backend team monitors stack, post-training team creates RL environments and benchmarks | [@Teknium on X](https://x.com/Teknium) |
| Multi-agent auto-build workflow (plan to code to QA to ship) — GPT-5.4 orchestrates, MiniMax M2.7 codes, local Qwen 35B tests | [@gkisokay on X](https://x.com/gkisokay) |
| Hermes as a watchdog for OpenClaw — saved hours every day | [@gkisokay on X](https://x.com/gkisokay) |
| Day 10: agent knows my codebase better than I do | X |
| Built my own stack, then converged on Hermes | X |
| Agent sees a file change and auto-acts on it | X |
| Codex watches Hermes agent-to-agent workflows live | [@gkisokay on X](https://x.com/gkisokay) |
| GLADIATOR: 9 Hermes agents, two rival AI companies competing via GitHub stars | YouTube |
| Telegram to Modal serverless — 40% faster on research tasks | Blog |
| 5 apps built and launched in a single day | LinkedIn |
| 8h/day on Opus: email pipeline with DBOS + Postgres + S3 | GitHub |
| Audited 129 of my own sessions across 23 days via external RCA script | GitHub |
| Skill Factory: silently watches workflows and writes SKILL.md + plugin.py | GitHub |
| CCD multi-agent pod on an M2 Ultra with Mem0 + Qdrant | GitHub |
| 73% of every API call is fixed overhead — built monitoring dashboard to profile token usage | GitHub |
| Accumulates knowledge about my codebase over time | Blog |
| **Kanban feature built into Hermes** — multi-agent kanban workflows for task orchestration | [u/itsdodobitch on Reddit](https://reddit.com/r/hermesagent/comments/1t4efcb/what_is_the_new_kanban_feature_built_into_hermes/) |
| **Agent Studio — Zero Human Company** — orchestration layer for team of engineer agents | [u/labeebk on Reddit](https://reddit.com/r/hermesagent/comments/1t5024g/agent_studio_zero_human_company/) |
| **Zero-token watchdog plugin** — monitors GitHub repos, RSS feeds, websites. Only notifies on changes | [u/JealousPlastic on Reddit](https://reddit.com/r/hermesagent/comments/1t5ibcc/i_built_a_zero-token_watchdog_plugin_for_hermes/) |
| **Self-hosted multi-agent system with 2 Hermes instances coordinating via Syncthing** — JARVIS orchestrator delegates to specialist workers | [u/SpecialistPowerful23 on Reddit](https://reddit.com/r/hermesagent/comments/1t4ltbx/i_built_a_selfhosted_multiagent_system_with_2/) |
| **Auto-generating agent skills from Apify Actors** — skill packages generated automatically from web scrapers | [u/Hayder_Germany on Reddit](https://reddit.com/r/hermesagent/comments/1t5p62c/experiment_autogenerating_agent_skills_from_apify/) |
| **Hermes Agent Self-Evolution (early alpha)** — DSPy + GEPA to automatically evolve Hermes skills | [u/piggystarter on Reddit](https://reddit.com/r/hermesagent/comments/1t5ifvg/nous_research_just_dropped_hermes_agent/) |
| **OpenCode integration plugin — dispatch coding tasks to multi-agent harness (17 stars)** | [zaycruz on GitHub](https://github.com/zaycruz/hermes-opencode-plugin) |
| **Hermes autonomous server — systemd + native cron, production-ready headless setup (7 stars)** | [JackTheGit on GitHub](https://github.com/JackTheGit/hermes-autonomous-server) |
| **Zo-oroboros swarm executors — Claude Code and Hermes integration (6 stars)** | [marlandoj on GitHub](https://github.com/marlandoj/zouroboros-swarm-executors) |
| **DeerMes — Deerflow execution layer + Hermes learning layer (2 stars)** | [Hompeaz on GitHub](https://github.com/Hompeaz/DeerMes) |
| **4 Anthropic-inspired agent features: Dreaming, Outcomes, Orchestration, Webhooks (PR #21212)** | [dashitongzhi on GitHub](https://github.com/NousResearch/hermes-agent/pull/21212) |
| **Hermes Agent Self-Evolution (2872 stars)** — optimize skills, prompts, and code using DSPy + GEPA | [NousResearch on GitHub](https://github.com/NousResearch/hermes-agent-self-evolution) |
| **Maestro — cross-agent coding conductor (151 stars)** — structured memory, handoffs, plan-approve-execute coordination | [ReinaMacCredy on GitHub](https://github.com/ReinaMacCredy/maestro) |
| **Lossless Context Management plugin (424 stars)** — DAG-based context engine that never loses a message | [stephenschoettler on GitHub](https://github.com/stephenschoettler/hermes-lcm) |
| **Hermes Labyrinth observability plugin (257 stars)** — journeys, crossings, guideposts, and reports | [stainlu on GitHub](https://github.com/stainlu/hermes-labyrinth) |
| **Icarus plugin — self-memory and replacement models (110 stars)** — remember your work, train your replacement | [esaradev on GitHub](https://github.com/esaradev/icarus-plugin) |
| **hermesctl — interactive control panel (18 stars)** — Bash + gum CLIProxy-style API menu | [byJoey on GitHub](https://github.com/byJoey/hermesctl) |
| **Hermes setup skill — deploy via coding agents (4 stars)** — Claude Code, Codex, OpenCode, Cursor, Windsurf | [hqhq1025 on GitHub](https://github.com/hqhq1025/hermes-setup-skill) |
| **Open Forge — AI-guided self-hosting for 950+ apps (38 stars)** — catalog self-improves with Hermes | [zhangqi444 on GitHub](https://github.com/zhangqi444/open-forge) |
| **hermescheck — architecture and runtime health checks (35 stars)** | [huangrichao2020 on GitHub](https://github.com/huangrichao2020/hermescheck) |
| **hermes-skills — bitwarden secrets, email composition, autoresearch loop (9 stars)** | [domvox on GitHub](https://github.com/domvox/hermes-skills) |
| **Agenvoy — agentic runtime with multi-provider concurrent dispatch, self-improving error memory, pluggable tools (83 stars)** | [agenvoy on GitHub](https://github.com/agenvoy/Agenvoy) |
| **hermes-nightshift-glm — autonomous overnight code quality bot (11 stars)** | [Microck on GitHub](https://github.com/Microck/hermes-nightshift-glm) |
| **pi-until-done — Pi extension that executes raw user intent to completion with verifiable termination (16 stars)** | [srinitude on GitHub](https://github.com/srinitude/pi-until-done) |
| **Ollama-Cloud-Hermes-Agent-Paperclip — route Paperclip agent swarm to Ollama Cloud models via resilient local proxy (1 star)** | [Vivere-Vitalis-LLC on GitHub](https://github.com/Vivere-Vitalis-LLC/Ollama-Cloud-Hermes-Agent-Paperclip) |
| **ClawFleet — deploy a fleet of AI agents on your machine in 10 minutes, use ChatGPT subscription (142 stars)** | [clawfleet on GitHub](https://github.com/clawfleet/ClawFleet) |
| **clawpier — desktop app for managing sandboxed AI agent instances via Docker, Tauri v2 (70 stars)** | [SebastianElvis on GitHub](https://github.com/SebastianElvis/clawpier) |
| **ClawEnvKit — open-source environment toolkit for claw-like agents, task/harness generation and evaluation (37 stars)** | [xirui-li on GitHub](https://github.com/xirui-li/ClawEnvKit) |
| **lowkey — deploy FullStack Coding Agent self-hosted in AWS Cloud (34 stars)** | [inceptionstack on GitHub](https://github.com/inceptionstack/lowkey) |
| **Ankh.md — multi-agent swarm framework (50 stars)** | [Abruptive on GitHub](https://github.com/Abruptive/Ankh.md) |
| **Network-AI — traffic light for AI Agents, TypeScript/Node multi-agent orchestrator with shared state, guardrails (45 stars)** | [Jovancoding on GitHub](https://github.com/Jovancoding/Network-AI) |
| **hermes-go — AI Agent framework in Go with RAG, Knowledge, Memory, Tools (25 stars)** | [Harsh-2909 on GitHub](https://github.com/Harsh-2909/hermes-go) |
| **bunny-agent — build coding agent SaaS via native AI SDK UI (15 stars)** | [buda-ai on GitHub](https://github.com/buda-ai/bunny-agent) |
| **agent-mux — unified TypeScript SDK and CLI for driving heterogeneous coding-agent harnesses (9 stars)** | [a5c-ai on GitHub](https://github.com/a5c-ai/agent-mux) |
| **tenbox — lightweight x86-64/arm64 Virtual Machine Monitor (VMM) for Hermes Agent (227 stars)** | [78 on GitHub](https://github.com/78/tenbox) |
| **super-hermes — skills that teach Hermes Agent to write its own analytical prompts (144 stars)** | [Cranot on GitHub](https://github.com/Cranot/super-hermes) |
| **hermes-dojo — self-improvement system that monitors performance, finds weak skills, fixes with self-evolution (53 stars)** | [Yonkoo11 on GitHub](https://github.com/Yonkoo11/hermes-dojo) |
| **hermes-gate — terminal TUI for managing remote Hermes Agent sessions with auto-reconnect, detach support (21 stars)** | [LehaoLin on GitHub](https://github.com/LehaoLin/hermes-gate) |
| **kali-pentest — AI agent skill for autonomous penetration testing via Kali Linux, 199 CLI tools, 15 scenario playbooks (10 stars)** | [x-glacier on GitHub](https://github.com/x-glacier/kali-pentest) |
| **hermes-swe-agent — autonomous AI coding agent triggered by Linear tickets, spins up full dev environments on EC2 (10 stars)** | [Deepank308 on GitHub](https://github.com/Deepank308/hermes-swe-agent) |
| **hermes-alpha — cloud deployed version of the Nous Research Hermes agent (194 stars)** | [kaminocorp on GitHub](https://github.com/kaminocorp/hermes-alpha) |
| **hermes-for-win — one-click installation and deployment of Hermes Agent and WebUI for Windows (23 stars)** | [EdwardWason on GitHub](https://github.com/EdwardWason/hermes-for-win) |
| **FlyEnv — lightweight native local dev toolbox for Windows/macOS/Linux, run Hermes/OpenClaw/n8n/Apache/Nginx (2787 stars)** | [xpf0000 on GitHub](https://github.com/xpf0000/FlyEnv) |
| **captureos — three-basket capture router for Hermes Agent (2 stars)** | [theali2x on GitHub](https://github.com/theali2x/captureos) |
| **cc-import — import Claude Code plugins (skills + agents) into Hermes Agent, translates personas into Hermes delegates (1 star)** | [roach88 on GitHub](https://github.com/roach88/cc-import) |
| **hermes-plugin-diff-review — plugin that reviews git diffs for common code quality issues (1 star)** | [sprmn24 on GitHub](https://github.com/sprmn24/hermes-plugin-diff-review) |
| **hermes-vscode — Hermes AI coding agent for VS Code, streams chat, runs tools, manages sessions, switches models (14 stars)** | [joaompfp on GitHub](https://github.com/joaompfp/hermes-vscode) |
| **hermes-agent-desktop — Multi-Agent AI Desktop Client, 20 specialists auto-collaborate on your tasks, Visual Skill Store, PM orchestrator (13 stars)** | [Felix-Forever on GitHub](https://github.com/Felix-Forever/hermes-agent-desktop) |

---

## Integrations (51 retained rows)

Connecting Hermes to external services: email, browsers, MCP tools, CRMs, home automation, and custom platforms.

| Use Case | Source |
|----------|--------|
| Give your Hermes its own email inbox — no extra services needed | X |
| Vessel Browser: agent-native browser born at the Hermes hackathon | Hacker News |
| Firecrawl for scrape/search/browse integration | LinkedIn |
| Hindsight Cloud memory connected | LinkedIn |
| Hermes + Browser Harness on a Hostinger VPS — copy-paste setup | GitHub Gist |
| Fat agent to thin tool provider via hermes mcp serve | GitHub Gist |
| jMunch MCP: 52 tools via tree-sitter for code intelligence | GitHub |
| Desktop computer-use module: noVNC, screenshots, mouse/keyboard | GitHub |
| Cross-agent memory: Hermes + Claude Code + Cursor | GitHub |
| Vercel Sandbox as a Hermes backend | GitHub |
| Webchat: custom themed browser UI on MEMORY.md | GitHub |
| Agent-to-agent commerce via Merxex | GitHub |
| Hermes in Zed editor via ACP Registry | GitHub |
| JMAP email for Fastmail users | GitHub |
| Home Assistant add-on: zero to agent in under 5 minutes | X |
| **One-Click Hermes Agent Container (Fox in the box)** — Docker container with PWA and mem0 persistent memory | [u/EmergencyCelery911 on Reddit](https://reddit.com/r/hermesagent/comments/1t4frvk/oneclick_hermes_agent_container_fox_in_the_box/) |
| **Hermes Client** — web UI for local Hermes CLI with usage dashboards and spending limits (100+ stars) | [u/lotsoftick on Reddit](https://reddit.com/r/hermesagent/comments/1t4amfs/hermes_client_a_web_ui_for_your_local_hermes_cli/) |
| **Prompt Vault plugin** — save, search, and reuse prompts from any platform | [u/JealousPlastic on Reddit](https://reddit.com/r/hermesagent/comments/1t4hrd4/i_built_a_prompt_vault_plugin_for_hermes_agent/) |
| **Mnemosyne: memory system for Hermes Agents** | [u/AbdiiSan on Reddit](https://reddit.com/r/hermesagent/comments/1t4ujs5/mnemosyne_a_memory_system_for_hermes_agents/) |
| **Hermes Memory Installer 2.0** — long-term memory with gbrain knowledge graph + PostgreSQL | [u/mage0535 on Reddit](https://reddit.com/r/hermesagent/comments/1t597dx/hermes_memory_installer_20_ai_longterm_memory/) |
| **HermesAgent in rootless podman containers** | [u/WouterC on Reddit](https://reddit.com/r/hermesagent/comments/1t4t5x2/hermesagent_in_rootless_podman_containers/) |
| **Home Assistant conversation integration for Hermes Agent (39 stars)** | [WolframRavenwolf on GitHub](https://github.com/WolframRavenwolf/hermes-ha-integration) |
| **Microsoft Graph API skill — Outlook Calendar, Mail, Contacts (7 stars)** | [Andrew-Girgis on GitHub](https://github.com/Andrew-Girgis/microsoft-workspace-skill) |
| **Eight Sleep Pod MCP server + Hermes integration (5 stars)** | [guglielmofonda on GitHub](https://github.com/guglielmofonda/8sleep-mcp) |
| **ApkClaw Android app with HTTP API for remote Agent control (4 stars)** | [rfdiosuao on GitHub](https://github.com/rfdiosuao/Hermes-Agent-phone) |
| **Anytype MCP integration skill (1 star)** | [Foolafroos on GitHub](https://github.com/Foolafroos/anytype-hermes-skill) |
| **Human-Like Memory skill for Hermes (1 star)** | [qwang6-1936520 on GitHub](https://github.com/qwang6-1936520/Human-like-memory-skill) |
| **OpenViking memory tool integration fork (1 star)** | [CUexter on GitHub](https://github.com/CUexter/hermes-agent) |
| **Steel cloud browser provider for Hermes (PR #21182)** | [nibzard on GitHub](https://github.com/NousResearch/hermes-agent/pull/21182) |
| **Hermes WebUI (6004 stars)** — best way to use Hermes Agent from the web or phone | [nesquena on GitHub](https://github.com/nesquena/hermes-webui) |
| **hermes-web-ui (3841 stars)** — multi-platform AI chat, session management, scheduled jobs, usage analytics | [EKKOLearnAI on GitHub](https://github.com/EKKOLearnAI/hermes-web-ui) |
| **Hermes workspace (3504 stars)** — native web workspace: chat, terminal, memory, skills, inspector | [outsourc-e on GitHub](https://github.com/outsourc-e/hermes-workspace) |
| **Hermes for web (64 stars)** — personalized web workbench with setup packs, artifacts, memory-aware onboarding | [reallygood83 on GitHub](https://github.com/reallygood83/hermes-for-web) |
| **Vessel Browser (63 stars)** — open source AI browser for Linux/Windows, durable state, MCP control, BYO model | [unmodeled-tyler on GitHub](https://github.com/unmodeled-tyler/vessel-browser) |
| **Hermes progress tail (2 stars)** — live tool and reasoning progress tails across Discord, Telegram | [tickernelz on GitHub](https://github.com/tickernelz/hermes-progress-tail) |
| **autograph — typed memory layer for always-on agents, schema-as-code for Obsidian (11 stars)** | [smixs on GitHub](https://github.com/smixs/autograph) |
| **hermes-console — local-first web dashboard: runtime health, sessions, cron, skills, memory, files (7 stars)** | [shan8851 on GitHub](https://github.com/shan8851/hermes-console) |
| **Hermes-iOS — camera, mic, health data, location, notifications for Hermes agents (6 stars)** | [dylan-buck on GitHub](https://github.com/dylan-buck/Hermes-iOS) |
| **hermes-google-workspace — Gmail, Calendar, Tasks, Sheets, Docs, Drive, Contacts via OAuth2 (2 stars)** | [BruceLanLan on GitHub](https://github.com/BruceLanLan/hermes-google-workspace) |
| **hermes-nextcloud — files, notes, calendar, tasks, contacts via WebDAV and CalDAV (2 stars)** | [adnw-vinc on GitHub](https://github.com/adnw-vinc/hermes-nextcloud) |
| **hermes-apple-calendar-assistant — Apple Calendar skill (1 star)** | [adoramon on GitHub](https://github.com/adoramon/hermes-apple-calendar-assistant) |
| **hermes-memory-keep-alive-for-obsidian — automatic task memory and keep-alive loop (14 stars)** | [TechieTer on GitHub](https://github.com/TechieTer/hermes-memory-keep-alive-for-obsidian) |
| **obsidian-vault-graph — local knowledge graph CLI with backlinks, centrality, QMD hybrid retrieval (3 stars)** | [kartikkabadi on GitHub](https://github.com/kartikkabadi/obsidian-vault-graph) |
| **aimx — SMTP for AI agents, no middleman (3 stars)** | [uzyn on GitHub](https://github.com/uzyn/aimx) |
| **hermes-android — Android device control, bridge app + Python toolset (137 stars)** | [raulvidis on GitHub](https://github.com/raulvidis/hermes-android) |
| **hermes-ui — command center: chat, steer, browse files, manage skills, monitor everything (102 stars)** | [pyrate-llama on GitHub](https://github.com/pyrate-llama/hermes-ui) |
| **hermes-control-interface — self-hosted web dashboard: terminal, file explorer, session overview (605 stars)** | [xaspx on GitHub](https://github.com/xaspx/hermes-control-interface) |
| **pan-ui — self-hosted AI workspace: chat, skills, extensions, memory, profiles, runtime controls (62 stars)** | [Euraika-Labs on GitHub](https://github.com/Euraika-Labs/pan-ui) |
| **OpenClaw-Admin — Vue 3 AI agent management platform supporting OpenClaw and Hermes (692 stars)** | [itq5 on GitHub](https://github.com/itq5/OpenClaw-Admin) |
| **hermes-weather-plugin — NWS-grade model imagery, NEXRAD radar, verified meteorological calculations in Rust (15 stars)** | [FahrenheitResearch on GitHub](https://github.com/FahrenheitResearch/hermes-weather-plugin) |
| **hermes-agent-win-gui — Windows 10/11 fork with browser-based web dashboard, sessions, config, chat, terminal (3 stars)** | [631341689 on GitHub](https://github.com/631341689/hermes-agent-win-gui) |

---

## Personal Assistant (22 retained rows)

Daily personal use: family assistants, home automation, health tracking, scheduling, and proactive AI helpers.

| Use Case | Source |
|----------|--------|
| One Hermes for the whole family on WhatsApp — 3 members, one $200 ChatGPT sub | [@EXM7777 on X](https://x.com/EXM7777) |
| Replaced everything with a single Hermes agent | X |
| Jarvis at home in 2026 — m2.7 + Hermes | X |
| Obsidian, home automation, VPS server management on a cheap VPS | Hacker News |
| Apple Health, Threads analytics, Gmail, Calendar in one CLI | Blog |
| Every weekday at 9am, summarize inbox and post to Slack | Blog |
| Hermes over iMessage on always-on Mac Studio | GitHub |
| Horse-racing Telegram community bot | GitHub |
| Hermes running on a Pi 4 as home server | GitHub |
| Bedtime stories for my daughter | GitHub |
| One agent, many roles: nutritionist, developer, finance advisor | GitHub |
| Proactive check-ins — "anything you want me to watch this afternoon?" | GitHub |
| Sometimes Hermes Agent melts my heart | X |
| Google Tasks integration | GitHub |
| Private Telegram topics, each with its own skill bindings | Blog |
| **SOUL.MD generator** — pick a template or describe your agent, get a personality file | [u/JealousPlastic on Reddit](https://reddit.com/r/hermesagent/comments/1t59352/i_built_a_soulmd_generator_for_hermes_pick_a/) |
| **Local-first wellness profile pack** — recovery, training, wellness tracking | [u/delxmobile on Reddit](https://reddit.com/r/hermesagent/comments/1t5jfol/i_built_a_localfirst_wellness_profile_pack_for/) |
| **24/7 personal AI agent on Galaxy Z Fold 6** — full-time agent living on phone | [u/fapas18 on Reddit](https://reddit.com/r/hermesagent/comments/1t4l28w/247_personal_ai_agent_running_on_a_galaxy_z_fold/) |
| **Multi-agent profile setup ("Archiver")** — specialized agents for different roles | [u/itsdodobitch on Reddit](https://reddit.com/r/hermesagent/comments/1t66lhy/my_simplest_yet_effective_hermes_agent_profile/) |
| **Personal API skill — turn your Obsidian vault into a personal identity layer, any AI agent knows who you are in 30s (14 stars)** | [beiyuii on GitHub](https://github.com/beiyuii/personal-api-skill) |
| **The Forge Protocol Agent — anti-deskilling framework, human-AI collaboration protocol (3 stars)** | [lorenzofamiglini on GitHub](https://github.com/lorenzofamiglini/The-Forge-Protocol-Agent) |
| **hermes_stack — personal AI agent setup with Hermes + gbrain + growing library of real-world skills (1 star)** | [kongaharsha on GitHub](https://github.com/kongaharsha/hermes_stack) |

---

## Meta & Ecosystem (28 retained rows)

Community tools, installers, migration guides, and ecosystem-building projects.

| Use Case | Source |
|----------|--------|
| Scraped the entire Hermes ecosystem (hermesatlas.com) | X |
| Show HN: independent install guide | Hacker News |
| Hermify: managed hosting for Hermes | [u/somratpro on Reddit](https://reddit.com/r/hermesagent/comments/1t3ne3a/run_hermes_agent_on_hugging_face_spaces_for_free/) |
| Native Windows app wrapper for Hermes | [Reddit](https://reddit.com/r/hermesagent) |
| hermes-for-win: one-click Windows installer | GitHub |
| Shadow-to-live migration path from OpenClaw | GitHub |
| Switched from OpenClaw, not looking back | X |
| Hermes Agent has won. Here's why. | Podcast |
| awesome-hermes-agent: community-curated skills list | GitHub |
| "The best self-improving agent we've used" | [Product Hunt](https://www.producthunt.com) |
| **Hermes Agent Wiki now live at hermesguide.xyz/wiki** — community wiki based on subreddit knowledge | [u/SelectionCalm70 on Reddit](https://reddit.com/r/hermesagent/comments/1t5k1t1/the_hermes_agent_wiki_for_subreddit_is_now_live/) |
| **awesome-hermes-agent: curated list of skills, tools, integrations (2654 stars)** | [0xNyk on GitHub](https://github.com/0xNyk/awesome-hermes-agent) |
| **Hermes Atlas — community map of every tool, skill, and integration (763 stars)** | [ksimback on GitHub](https://github.com/ksimback/hermes-ecosystem) |
| **awesome-hermes-skills: production-ready skills collection (49 stars)** | [ChuckSRQ on GitHub](https://github.com/ChuckSRQ/awesome-hermes-skills) |
| **Hermes optimization guide (263 stars)** — setup, migration, LightRAG, Telegram, skill creation | [OnlyTerp on GitHub](https://github.com/OnlyTerp/hermes-optimization-guide) |
| **hermes-skills — 310+ reusable AI agent workflows for coding, marketing, design, finance, MLOps (5 stars)** | [itgoyo on GitHub](https://github.com/itgoyo/hermes-skills) |
| **Hermes setup guide — local AI model, Telegram bot, TurboQuant acceleration (1 star)** | [JulCCrum on GitHub](https://github.com/JulCCrum/hermes-setup-guide) |
| **learn-hermes-agent — 27-chapter hands-on tutorial for building autonomous AI agent from zero (87 stars)** | [longyunfeigu on GitHub](https://github.com/longyunfeigu/learn-hermes-agent) |
| **memo-agent — terminal-based AI assistant (Hermes simplified), TypeScript + React (5 stars)** | [lxfu1 on GitHub](https://github.com/lxfu1/memo-agent) |
| **awesome-HermesAgent-tutorial — comprehensive tutorial: learning loops, cross-platform messaging, 200+ model routing (6 stars)** | [xianyu110 on GitHub](https://github.com/xianyu110/awesome-HermesAgent-tutorial) |
| **alblez/hermes-skills — collection of reusable skills (10 stars)** | [alblez on GitHub](https://github.com/alblez/hermes-skills) |
| **hurmoz — 63 Arabic AI skills for Hermes Agent, first and largest Arabic skills collection (6 stars)** | [Moshe-ship on GitHub](https://github.com/Moshe-ship/hurmoz) |
| **hermes-howto — comprehensive step-by-step guide and best practices for mastering Hermes Agent (5 stars)** | [1234567mm on GitHub](https://github.com/1234567mm/hermes-howto) |
| **agency-agents-zh — 211 plug-and-play AI expert roles, supports Hermes/Claude Code/Cursor/Copilot, 18 departments (10084 stars)** | [jnMetaCode on GitHub](https://github.com/jnMetaCode/agency-agents-zh) |
| **superpowers-zh — AI programming superpowers Chinese enhanced edition, 116k+ stars original + 6 China-original skills (2198 stars)** | [jnMetaCode on GitHub](https://github.com/jnMetaCode/superpowers-zh) |
| **hermes-skill-marketplace — self-evolving Hermes agent that writes, tests, and publishes reusable Skills autonomously (13 stars)** | [Lethe044 on GitHub](https://github.com/Lethe044/hermes-skill-marketplace) |
| **awesome-hermes-skills — curated install-ready skills for Hermes Agent, 85 built-in + 78 community skills, plugins, and tools (5 stars)** | [ZeroPointRepo on GitHub](https://github.com/ZeroPointRepo/awesome-hermes-skills) |
| **awesome-hermes-usecases — curated real-world use cases for Hermes Agent backed by primary sources (31 stars)** | [aliaihub on GitHub](https://github.com/aliaihub/awesome-hermes-usecases) |

---

## Business Ops (17 retained rows)

Client management, CRM, sales outreach, inventory tracking, and business process automation.

| Use Case | Source |
|----------|--------|
| Client research, follow-ups, podcasts, leads — all on Hermes | X |
| Live inventory tracking on Hermes with 40+ built-in tools | X |
| Day 297 of streak: $100K of client work automated | X |
| Auto-transcribe Meet calls, control from Teams, local models for client data | Blog |
| 24/7 assistant with a Supabase CRM, built in a demo | YouTube |
| Task-centric memory for a printing factory | GitHub |
| **B2B SDR skill (7 stars)** — autonomous AI sales dev rep: finds buyers, qualifies leads, sends outreach, manages CRM across WhatsApp/Email/Telegram | [iPythoning on GitHub](https://github.com/iPythoning/b2b-sdr-hermes-skill) |
| Create and edit Google Slides decks via google-workspace skill | GitHub |
| Hunter.io email-finding for sales outreach via Composio | GitHub |
| **provision-core — open-source AI workforce platform: agents with tasks, tools, browsers, email (17 stars)** | [provision-org on GitHub](https://github.com/provision-org/provision-core) |
| **discord-meeting-recorder — self-hosted Discord meeting transcription with AI-generated reports (1 star)** | [elghaied on GitHub](https://github.com/elghaied/discord-meeting-recorder) |
| **hermes-legal — autonomous contract risk analysis, scores clauses, suggests negotiation language (3 stars)** | [Lethe044 on GitHub](https://github.com/Lethe044/hermes-legal) |
| **my-pretty-decent-skills — battle-tested multi-agent workflows for supply chain threat intel (1 star)** | [AI-1409 on GitHub](https://github.com/AI-1409/my-pretty-decent-skills) |
| **hermes-merchant — portable agent skills that scrape ML/AI jobs, score against profile, auto-fill Greenhouse applications (28 stars)** | [arimanyus on GitHub](https://github.com/arimanyus/hermes-merchant) |
| **hermes-pm-toolkit — self-writing skills for PMs running on Hermes agent runtime, competitive analysis workflows (5 stars)** | [aakashg on GitHub](https://github.com/aakashg/hermes-pm-toolkit) |
| **h-ops — Hermes Agent operations cockpit for Kanban: health, assignment, progress, logs, output, run history (2 stars)** | [tmdgusya on GitHub](https://github.com/tmdgusya/h-ops) |
| **agent-team-orchestrator — document-first multi-agent product organization pack for Hermes with role boundaries, handoff contracts, review gates (1 star)** | [DeclanJeon on GitHub](https://github.com/DeclanJeon/agent-team-orchestrator) |

---

## Enterprise (7 retained rows)

Production deployments, compliance, Kubernetes, GCP Vertex AI, and enterprise-grade infrastructure.

| Use Case | Source |
|----------|--------|
| Hermes inside an MCP infrastructure behind Higress | GitHub |
| EU AI Act compliance via Ombre — tamper-proof audit trail | GitHub |
| Kubernetes pod-hop handoff across restarts | GitHub |
| Vertex AI for GCP-standardized enterprises | GitHub |
| Hermes as CLI/gateway-first — 13 platforms under one process | Blog |
| Azure-compliant prompt patch so the safety filter doesn't kick in | GitHub Gist |
| **hermes-multitenancy — one Feishu bot, N users, N profiles, multi-tenant routing plugin (2 stars)** | [eggyrooch-blip on GitHub](https://github.com/eggyrooch-blip/hermes-multitenancy) |

---

## Content Creation (8 retained rows)

Voice-matched writing, YouTube scripts, social media content, and creative publishing workflows.

| Use Case | Source |
|----------|--------|
| Monica that writes in my voice | X |
| Scraped Amazon without extra config; built a YouTube title skill | YouTube |
| Tweets in my voice, pulled from past video scripts | YouTube |
| **YouTube skills (174 stars)** — transcript API, video search, channel browsing for AI agents | [ZeroPointRepo on GitHub](https://github.com/ZeroPointRepo/youtube-skills) |
| Weekly cron: top 3 trending AI tools for next video | YouTube |
| LinkedIn posts that remember my style | YouTube |
| **obsidian-video-notes — video-to-note workflow, transcribes audio via faster-whisper, generates structured Markdown (2 stars)** | [lomychenbao-bit on GitHub](https://github.com/lomychenbao-bit/obsidian-video-notes) |
| **Noustiny — agent native video creation pipeline on top of Hermes Agent (75 stars)** | [UfukNode on GitHub](https://github.com/UfukNode/Noustiny) |

---

## Cost Optimization (6 retained rows)

Running Hermes cheap: VPS setups, model selection, token reduction, and budget hosting.

| Use Case | Source |
|----------|--------|
| Under $20/mo total — no Mac Mini, no Opus | Blog |
| Hetzner VPS at $10/mo, Claude Opus via OpenRouter | YouTube |
| 90% token spend cut. Runs on a cheap Android via Termux | Podcast |
| $5 VPS playbook so the defaults don't eat your OpenRouter budget | Blog |
| **Run Hermes Agent on Hugging Face Spaces for FREE (24/7)** | [u/somratpro on Reddit](https://reddit.com/r/hermesagent/comments/1t3ne3a/run_hermes_agent_on_hugging_face_spaces_for_free/) |
| **LLM keypool (24 stars)** — free-tier API key pool with rotation, cooldown handling, OpenAI-compatible proxy | [piyush-tyagi-13 on GitHub](https://github.com/piyush-tyagi-13/llm-keypool) |

---

## Creative (21 retained rows)

Video generation, generative art, music, and creative media production.

| Use Case | Source |
|----------|--------|
| My Hermes agent makes movies now — browser_use + Seedance 2.0, no API needed | [@alexcovo_eth on X](https://x.com/alexcovo_eth) |
| shadcn finance dashboard + Manim explainer videos | YouTube |
| Generative visuals in TouchDesigner via Hermes skill | GitHub |
| **Hermes Agent generates full videos with HyperFrames** — HTML/CSS/JS/GSAP animations rendered to MP4 | [u/SelectionCalm70 on Reddit](https://reddit.com/r/hermesagent/comments/1t4kcup/hermes_agent_now_generates_full_videos_with/) |
| **The Holographic Mind: AI that thinks, feels, and writes philosophy** — two-week build | [u/petriko on Reddit](https://reddit.com/r/hermesagent/comments/1t4meas/the_holographic_mind_how_i_built_an_ai_that/) |
| **Hermes Agent integration for Open-LLM-VTuber (3 stars)** | [123mikeyd on GitHub](https://github.com/123mikeyd/hermes-vtuber) |
| **Codex imagegen skill (22 stars)** — generate images directly in Hermes using ChatGPT Codex CLI | [madrobotnet on GitHub](https://github.com/madrobotnet/hermes-codex-imagegen-skill) |
| **drawio-skill — from text to professional diagrams, generates draw.io diagrams from natural language (1254 stars)** | [Agents365-ai on GitHub](https://github.com/Agents365-ai/drawio-skill) |
| **visual-skills — professional AI image and video prompting for Gemini 3 Pro/Flash, GPT Image 2 (26 stars)** | [smixs on GitHub](https://github.com/smixs/visual-skills) |
| **Wizards-of-the-Ghosts — unofficial Hermes Agent skill pack built from fantasy spell and skill names (77 stars)** | [Hmbown on GitHub](https://github.com/Hmbown/Wizards-of-the-Ghosts) |
| **hermes-music-plugin — music generation plugin: Suno AI, MIDI composition, library management (5 stars)** | [buckster123 on GitHub](https://github.com/buckster123/hermes-music-plugin) |
| **meme-library — meme library skill with visual auto-tagging + semantic search, agent picks perfect meme on demand (1 star)** | [yr-96 on GitHub](https://github.com/yr-96/meme-library) |
| **Heimdall-SL-Hermes-Agent — autonomous Second Life Agent for the Gridweaver (2 stars)** | [hrabanazviking on GitHub](https://github.com/hrabanazviking/Heimdall-SL-Hermes-Agent) |
| **DocFlow-Presentations-and-Docs-Skill — agent-first Python skill for Hermes & OpenClaw: DOCX/XLSX/PDF/PPTX via OfficeSuite, HTML template catalog (1 star)** | [rafalozan0 on GitHub](https://github.com/rafalozan0/DocFlow-Presentations-and-Docs-Skill) |
| **crimson-desert-companion — complete Crimson Desert game database & AI companion, 9,288 rows, 34 query actions, Hermes skill (1 star)** | [b7216309-jpg on GitHub](https://github.com/b7216309-jpg/crimson-desert-companion) |
| **hermes-embodied — self-improving robotics via Hermes Agent, provisions cloud GPUs, fine-tunes VLA models, runs sim evaluation (6 stars)** | [bryercowan on GitHub](https://github.com/bryercowan/hermes-embodied) |
| **caduceus-robot — Hermes Agent's physical form, voice-controlled robot for the Hermes Agent Creative Hackathon (4 stars)** | [devorun on GitHub](https://github.com/devorun/caduceus-robot) |
| **robot-bridge — StackChan Robot Bridge, connect ESP32 to Hermes Agent (1 star)** | [waynecc-at on GitHub](https://github.com/waynecc-at/robot-bridge) |
| **waifu-sprites — Codex Pets for hermes-agent, dashboard plugin with multi-pet support (12 stars)** | [waifuai on GitHub](https://github.com/waifuai/waifu-sprites) |
| **agent-pet — lightweight desktop companion for AI tools and agent activity, supports Codex-compatible sprite sheets (11 stars)** | [xiangking on GitHub](https://github.com/xiangking/agent-pet) |
| **hermes-game-dev-studio — turn Hermes Agent into a full game development studio (1 star)** | [laok775 on GitHub](https://github.com/laok775/hermes-game-dev-studio) |

---

## Research (9 retained rows)

Research agents, knowledge bases, academic tools, and information synthesis.

| Use Case | Source |
|----------|--------|
| Daily research brief across Discord, Slack, Notion & Obsidian — watches AI/agent space | [@gkisokay on X](https://x.com/gkisokay) |
| I had my research agent dig into what people are building with Hermes | [Reddit](https://reddit.com/r/hermesagent) |
| A self-improving LLM Wiki second brain — knowledge base that compounds over time | Blog |
| LaTeX math renders properly in the TUI | GitHub |
| **Agentic SWMM workflow — stormwater modeling with QA verification (4 stars)** | [Zhonghao1995 on GitHub](https://github.com/Zhonghao1995/agentic-swmm-workflow) |
| **Hermes web search plus (156 stars)** — multi-provider web search with intelligent routing, quality reports, research mode | [robbyczgw-cla on GitHub](https://github.com/robbyczgw-cla/hermes-web-search-plus) |
| **hermes-agent_Video-Research-Ingest — local-first video research ingest pipeline, videos and URLs into markdown notes (historical entry)** | Repository returned 404 when checked July 16, 2026; retained for provenance pending a corrected source |
| **hermes-weather-agent — MCP tools for AI-driven weather model training, rustmet + metrust powered (7 stars)** | [FahrenheitResearch on GitHub](https://github.com/FahrenheitResearch/hermes-weather-agent) |
| **askaipods — search AI podcast quotes by topic, find what Lex Fridman, Dwarkesh Patel, No Priors guests said (2 stars)** | [Delibread0601 on GitHub](https://github.com/Delibread0601/askaipods) |

---

## Messaging (7 retained rows)

Platform adapters for LINE, QQ, Feishu/Lark, Discord, and regional messaging apps.

| Use Case | Source |
|----------|--------|
| LINE for 95M+ users in Japan | GitHub |
| QQ Bot adapter for China | GitHub |
| Give Hermes hands inside Feishu (Lark) — full ecosystem coverage | GitHub |
| DM-based approval gate for kid-facing Discord bots | GitHub |
| **WeChat messaging integration with cron delivery** | [NousResearch on GitHub](https://github.com/NousResearch/hermes-agent/pull/21196) |
| **OctoMatrix — autonomous AI command center for Telegram, Discord & Slack with long-term memory via grep-based RAG (1 star)** | [meso4444 on GitHub](https://github.com/meso4444/OctoMatrix) |
| **hermes-agent-qq-gateway — standalone QQ Official Bot gateway (1 star)** | [zhaoxuya520 on GitHub](https://github.com/zhaoxuya520/hermes-agent-qq-gateway) |

---

---

## Comment-Sourced Updates

- **Link integrity:** commenters reported broken links; the author later audited and corrected/removed links. This repository copy still requires automated link checking because external targets continue to change.

- **Outcome claims:** revenue, throughput, time-saved, and business-result numbers are community reports unless a primary source or reproducible artifact is provided.

- **Make entries actionable:** comments favored SOP-style entries with the problem, tools, steps, output, and source rather than an undifferentiated list. Use issues/PRs to improve individual entries.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
