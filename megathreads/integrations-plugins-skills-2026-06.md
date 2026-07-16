# Integrations, Plugins & Skills Ecosystem Megathread — Hermes Agent (June 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1ui5a91/integrations_plugins_skills_ecosystem_megathread/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

**LAST UPDATED: June 28, 2026** | **Scope: Late April – June 28, 2026 (includes threads from today) | ~42 threads analyzed** | **Megathread — Reference resource**

This is the community's collective knowledge on connecting Hermes Agent to everything else — messaging platforms, productivity apps, home automation, developer tools, MCP servers, webhooks, and the Skills Hub. Built from subreddit discussions, X/Twitter, and official Hermes docs.

---

## TL;DR — Quick Reference

| Use Case | Community Pick | Runner-Up | Notes |
|----------|---------------|-----------|-------|
| Multi-platform messaging | Telegram (native gateway) | Discord | WhatsApp (official adapter in v0.17.0), Signal, iMessage (Photon Spectrum), SimpleX |
| Notes & knowledge base | Obsidian vault skill | Notion API | Obsidian wins on local-first + markdown |
| Email | Himalaya CLI (IMAP/SMTP) | Google Workspace for agent | Personal Gmail risky — Google bans bot activity |
| Calendar | Google Calendar via gws CLI | Apple Calendar (macOS) | Cron + calendar = automated scheduling |
| Home automation | Home Assistant custom integration | MQTT bridge | HA add-on v1.1.0 supports multi-profile |
| MCP server management | Built-in MCP Catalog | Manual stdio config | Catalog = one-click install for Nous-approved MCPs |
| API/webhook automation | Webhooks adapter + GitHub | n8n + Hermes | Webhooks support HMAC signature validation |
| Secret management | MCP + local password manager | `~/.hermes/.env` | Never store API keys in plaintext config |
| Skill discovery | Skills Hub (90K+ skills) | Subreddit showcases | Quality varies — check recency and reviews |
| Browser automation | Browserbase web skill library | Built-in browser tools | External web skill library gaining traction |
| E-commerce / business | Custom MCP tools (Shopify, Amazon) | n8n workflows | Most community-built, not off-the-shelf |

---

## Part 1: Messaging Platforms — Where Hermes Lives

Hermes Agent ships with native gateway support for Telegram, Discord, WhatsApp, Signal, and email. Setup is straightforward through the dashboard, but the community has surfaced key patterns and pitfalls.

### Telegram (Community Favorite)

By far the most-used messaging platform. Key findings from the subreddit:

- **Setup:** Native gateway via `hermes gateway telegram`. Most users run this on a VPS for 24/7 availability.
- **Multiple bots, one instance:** Users have successfully run multiple Telegram bots from one Hermes instance by configuring separate profiles with per-profile API tokens. [thread: "Multiple Telegram Bots on VPS with one Hermes Instance?", May 18](https://www.reddit.com/r/hermesagent/comments/1tg5edt/)
- **Message formatting:** Hermes can struggle with Telegram's markdown formatting — specifically code blocks and tables. Workaround: configure the agent to use simpler formatting or pre-process output. [thread: "Can't get it to fix the format of telegram messages", May 27](https://www.reddit.com/r/hermesagent/comments/1tpoeht/)
- **Group chat history:** Hermes doesn't automatically see prior chat history in Telegram groups. Explicitly reference or quote context if needed. [thread: "Hermes doesn't see chat history in Telegram groups", May 19](https://www.reddit.com/r/hermesagent/comments/1tiqg88/)

### Discord

- Gateway setup similar to Telegram. Less discussed but well-supported.
- Context is shared across platforms — same agent, same memory, different UI. [thread: "Is context shared across messaging platforms?", Apr](https://www.reddit.com/r/hermesagent/comments/1slajqe/)

### WhatsApp & Signal

- Available via gateway. Lower community volume but functional.
- **v0.17.0 (Jun 19): Official WhatsApp Business Cloud API adapter** — first-party, hosted, no bridge process. Alongside the existing Baileys bridge. `hermes gateway whatsapp` for setup.
- Setup through dashboard — similar pattern to Telegram/Discord.

**Community Spotlight — WhatsApp in Production (Jun 28, 2026):**
A community member shared a real production setup using OpenWA (self-hosted WhatsApp API) + Hermes Agent to manage 11 construction WhatsApp groups:
- Hermes reads all group messages (tower crane updates, QA/QC reports, manpower tracking, safety alerts)
- Summarizes 82 WhatsApp messages into 3 lines: *"L970 Tower Crane: 52 lifts, TC 2 dominant. Jacking postponed — hydraulic issue. QA/QC: 23+2 workers, Block A L6-L7 vent block ongoing."*
- Sends scheduled messages (e.g., check rebar balance, follow up on insurance)
- Stack: OpenWA (self-hosted on laptop, localhost) → Hermes Agent (REST API) → Cronjobs → Telegram control interface
- Runs on 8GB laptop, no monthly SaaS fees, no cloud dependency
- Natural language in Manglish: *"Check L970 groups ada apa update hari ni."* Hermes understands context.
- Community questions focused on hallucination prevention and WhatsApp ToS compliance (OpenWA is not officially sanctioned — the new official Business Cloud API adapter is the first-party alternative)

[thread: "I turned Hermes Agent into my construction site assistant. It now manages my WhatsApp.", 35 upvotes, 11 comments, Jun 28](https://www.reddit.com/r/hermesagent/comments/1uhyift/)

### iMessage (New in v0.17.0)

- **Photon Spectrum — no Mac relay required.** `hermes photon login` (device-code OAuth), gRPC-native channel, markdown rendering, emoji reactions, outbound media. This replaces the old macOS-only `imsg` CLI approach.
- Previously required a Mac running the `imsg` CLI — now available on any platform.

### SimpleX (New in v0.17.0)

- Groups, native attachments, text batching, auto-accept. Bundled platform plugin.
- Privacy-focused messaging with no phone number required.

### Email

- **Himalaya CLI** is the community's preferred IMAP/SMTP tool — works well for agent-driven email.
- **Gmail risk:** Multiple users report Google banning accounts used by Hermes for bot-like activity. Strong consensus: use Google Workspace (paid) for the agent's email, not a free personal Gmail. One user: "after one day Google blocked a gmail account I made for Hermes." [thread: "Gmail banned with hermes why!?!", May 27](https://www.reddit.com/r/hermesagent/comments/1tpgwp7/)
- Dedicated email provider (e.g., Fastmail, Proton Mail with bridge) recommended for agent-only accounts.

### Community Preference Poll

From the "Which messaging channel do you use?" thread (Apr 29):
- Telegram: dominant
- Discord: second
- WhatsApp/Signal: smaller but growing
- Email: niche (used for specific workflows, not primary chat)

---

## Part 2: Productivity & Knowledge Integrations

### Obsidian — The Community Standard for Knowledge Management

The [most-engaged post](https://www.reddit.com/r/hermesagent/comments/1stz6gd/) in r/hermesagent history (1,029 upvotes, 179 comments, Apr 24) is a comprehensive Obsidian-as-memory-backbone guide. The core architecture that resonated with the community:

**Three-Tier Memory System:**
- **Tier 1 — Hot Memory:** Per-session context (~9K chars). When it hits ~67% capacity, stable entries get promoted to vault files.
- **Tier 2 — Vault Living Files:** Stable reference material (environment configs, known failure patterns). Agent reads on-demand.
- **Tier 3 — Daily Notes:** `Daily/YYYY-MM-DD.md` with tasks, schedule, log, wins. Searchable decision history.

**Key patterns from the 179 comments:**
- SyncThing integration for syncing VPS work folders to local Obsidian
- Cross-agent memory: users running Hermes + Claude Code pointed at the same vault path
- Windows PowerShell version of the scaffold script posted and tested

**Obsidian vs Notion:** Community consensus favors Obsidian — local markdown files, no API rate limits, wiki-links create connection graphs. Notion cited for collaboration but API rate limiting remains an issue. [thread: "Obsidian or Notion", Apr 29](https://www.reddit.com/r/hermesagent/comments/1syvg1e/)

### Notion

- Notion API skill available (`notion` skill) — pages, databases, markdown, Workers.
- Users who prefer Notion cite better collaboration features and rich database views.
- Rate limiting and API latency are the main complaints vs Obsidian's local files.

### Google Workspace

- **Gmail:** gws CLI skill for reading/sending email. Works well with Google Workspace accounts. Free Gmail = ban risk.
- **Calendar:** Google Calendar via gws CLI. Common pattern: cron job checks calendar → Hermes summarizes day ahead.
- **Drive:** File management via gws CLI. Used for document storage and retrieval.
- **Sheets/Docs:** Read/write via gws CLI. Used for expense tracking, reporting, data logging.

### Apple Ecosystem

- **Apple Notes:** `memo` CLI skill available. Community reports occasional issues with the skill.
- **Apple Reminders:** `remindctl` CLI skill — add, list, complete.
- **iMessage:** Photon Spectrum plugin — `hermes photon login` (device-code OAuth). No Mac relay required (new in v0.17.0). The old `imsg` CLI remains available for macOS users.

### Other Productivity Tools

- **Todoist:** API integration for task management. Mentioned in Obsidian thread as external data source.
- **Excel/Spreadsheets:** Hermes can read/write `.xlsx` files natively. Users report permission prompts for financial data files — the safety layer flags these. [thread: "Hermes keeps asking permission to read/write my expense Excel", Jun 1](https://www.reddit.com/r/hermesagent/comments/1tu458c/)
- **Himalaya CLI:** IMAP/SMTP email client, preferred for agent-driven email workflows.

---

## Part 3: Home Automation & IoT

### Home Assistant (HA)

The dominant home automation platform for Hermes users. Active community development:

- **Official add-on:** WolframRvnwlf maintains a Hermes Agent Home Assistant Add-on that takes you "from zero to working agent in less than 5 minutes." [X: @WolframRvnwlf, Mar 27]
- **v1.1.0 (Jun 1, 2026):** Added multi-profile support — run multiple Hermes agents side by side with per-profile env, dashboard, terminal, and API routes.
- **Voice loop:** Full wake-word → STT → Hermes → TTS → speaker pipeline. Custom integration connects Hermes as a native Conversation Agent in HA. [X: @WolframRvnwlf, Apr 4]
- **Limitations:** Hermes can read HA entity states but cannot directly edit automations through the standard integration. Community workaround: use YAML file editing or MQTT-based approaches. [thread: "Hermes can't edit Home Assistant automations", Apr 30](https://www.reddit.com/r/hermesagent/comments/1szxyr4/)

### HA vs Bespoke

Community consensus from the "Hermes home automation: use Home Assistant or bespoke?" thread (Jun 16):
- **Use Home Assistant** for most home automation tasks — "it is far more mature than any of the agent tools, and the majority of all home automation tasks are better handled by HA directly."
- **Use Hermes** as the intelligence layer on top — natural language control, scheduling, anomaly detection, and multi-step automation coordination.
- **Do NOT** try to replace HA with Hermes — the integrations ecosystem (Zigbee, Z-Wave, Matter, 3000+ devices) is irreplaceable.

### Other IoT Patterns

- **MQTT bridge:** Direct MQTT topic subscription for sensor data ingestion.
- **Camera feeds:** Hermes can receive camera snapshots via webhooks or file monitoring.
- **Sensor data logging:** Cron job reads HA entity states → writes to Obsidian vault or database.

---

## Part 4: Developer Tools & MCP

### MCP (Model Context Protocol) — The Extension Backbone

MCP is Hermes's primary mechanism for connecting to external tool servers. Key facts from the official docs and community:

- **Built-in MCP Catalog:** One-click install for Nous-approved MCP servers. Commands: `hermes mcp` (interactive picker), `hermes mcp install <name>` (install by name). [X: @NousResearch, May 27 — 107 replies]
- **Two server types:** Stdio (local subprocesses) and HTTP (remote endpoints with OAuth support).
- **OAuth 2.1 support:** Linear, Sentry, Atlassian, Asana, Figma, Stripe — tokens cached at `~/.hermes/mcp-tokens/`.
- **Tool selection at install:** Interactive checklist — pick which specific tools to expose to the agent.
- **Trust model:** Catalog entries gated by PR review into the hermes-agent repo. Always read the manifest's `source:` and `install.bootstrap:` fields.

### Community-Built MCP Tools

- **E-commerce MCP:** Shopify, Amazon, and Google Maps tools for product intelligence, inventory, and location data. Built by a community member. [thread: "Built 3 MCP tools for e-commerce intelligence"](https://www.reddit.com/r/hermesagent/comments/1srp2wt/)
- **Web analytics plugin:** Multi-project analytics dashboard plugin. [thread: "Built a Hermes plugin for multi-project web analytics", Apr 21](https://www.reddit.com/r/hermesagent/comments/1srp2wt/)
- **n8n + Hermes + MCP:** Community setup combining n8n for visual workflow automation with Hermes MCP for intelligent tool selection. [thread: "I built a complete Hermes Agent Desktop setup with MCPs, voice mode and n8n", Jun ~10](https://www.reddit.com/r/hermesagent/comments/1u245ua/)
- **xurl CLI:** X Developers published official guide to connect Hermes with xurl for X/Twitter integration via xAI's Grok. [X: May 20]

### Webhooks

Hermes ships a webhook adapter that:
- Runs an HTTP server accepting POST requests
- Validates HMAC signatures
- Transforms payloads into agent prompts
- Routes responses back to source or another platform

Common use cases: GitHub PR notifications → Hermes review, Stripe payment events → agent logging, JIRA ticket updates → agent response.

### GitHub Integration

- **gh CLI:** Full GitHub workflow — clone, create PRs, review code, manage issues.
- **Webhooks:** GitHub → Hermes for automated PR review, issue triage, CI/CD notifications.
- **MCP GitHub server:** Stdio-based server for deeper GitHub API access.

---

## Part 5: The Skills Ecosystem

The Skills Hub launched in April 2026 and has exploded. As of June 2026: **90,000+ skills available**. The community has shifted from basic chat to skill-driven automation.

### Skills Hub Overview

- **Discovery:** Browse and install skills from the Skills Hub (dashboard or `hermes skills` CLI).
- **Quality signals:** Recency, install count, author reputation, and community reviews help filter noise. No centralized rating system — rely on subreddit recommendations.
- **Self-evolving skills:** Hermes can create and improve its own skills based on experience. The skill-audit pattern (review → patch → test) is the community standard.

### Community Favorite Skills

From the "10 skills" community thread (Jun 20) and wider subreddit discussion, the most frequently mentioned and endorsed skills:

- **`/skill-creator`** — The #1 most-upvoted recommendation (8 points). A built-in skill that lets Hermes create new skills dynamically. Community consensus: "start here, let Hermes build what you need."
- **humanizer** — Built-in skill that makes AI-generated text more natural. Frequently used by content creators.
- **github-pr-workflow / github-repo-management** — Built-in skills for GitHub automation. Used by developers running CI/CD through Hermes.
- **Weather** — Simple but cited as surprisingly useful: "I use it all the time, surprisingly." Good example of a skill that earns its keep through frequency.
- **blog-publisher** — Custom community skill for automated blog publishing workflows.
- **grill with docs** — Custom skill for interrogating/querying documentation sources.
- **computer-use** — Desktop control skill (macOS background driving).
- **obsidian / apple-notes / notion** — Knowledge management skills, with Obsidian being the community favorite.

**Community philosophy on skills:** The most-upvoted takeaway from the thread — "The ones you need to achieve your daily goals or tasks, no more, no less." Start minimal; add only what earns its context budget.

### Building Your Own Skills

Key community patterns:
1. **Start simple:** A single SKILL.md with frontmatter + markdown body. Use `/skill-creator` to scaffold.
2. **Let Hermes iterate:** Describe what you want; the agent generates a skill and improves it through use.
3. **Audit regularly:** Skills can drift. The "Hermes Skill Audit" workshop (Jun 7) covers why skills stop firing and how to fix them — stale trigger conditions, path assumptions that changed, model-switching side effects.
4. **Share selectively:** Not every skill needs to be shared. Polish the ones that solve real problems.

### Skill Packs & Multi-Skill Bundles

A community member built and shared 7 ready-to-install skill/plugin packs (May 30) covering:
- **Auto-install skill** — A meta-skill that installs other skills programmatically
- **Productivity bundle** — Task management, scheduling, note-taking grouped skills
- **Development tools** — Git workflow, code review, project management skills
- Additional packs for content creation, automation, and system monitoring

The auto-install pattern is notable: it reduces the friction of "find, download, configure" to a single command, making skill discovery and adoption faster.

### Known Issues

- **Skill curator:** Community previously reported that the skill curator feature had issues with stale metadata. **Partially addressed in v0.17.0 (Jun 19):** The curator now prunes stale skills by default but no longer runs its LLM-powered consolidation pass unless opted in (`curator_consolidate: true`), eliminating aux-model spend on routine runs. [thread: "The skill curator feature in Hermes Agent has a big issue", May 11](https://www.reddit.com/r/hermesagent/comments/1t9smfc/)
- **Skills not firing:** Common causes: trigger wording mismatch, path assumptions broken after env changes, model switches that drop skill context. Audit workflow: check frontmatter triggers → verify file paths → test with small prompt.
- **Skill review:** Some users report skill review/approval delays or unclear status. [thread: "Skill review issue!", May 8](https://www.reddit.com/r/hermesagent/comments/1t71x8c/)

---

## Part 6: API Connections, Webhooks & Automation Patterns

### Authentication Patterns

The community has settled on three main approaches:

1. **API keys in `.env`:** Store in `~/.hermes/.env`, reference in config as `${VAR}`. Better than plaintext in config.yaml but still plaintext on disk.
2. **MCP with OAuth:** For supported providers (Linear, Sentry, Stripe, etc.), MCP's built-in OAuth 2.1 flow handles token exchange, refresh, and secure caching at `~/.hermes/mcp-tokens/`.
3. **Local password manager + MCP server:** Community-built solution that wraps a local password manager (e.g., Bitwarden CLI, `pass`) in an MCP server so Hermes retrieves secrets at runtime without storing them in plaintext. [thread: "Stop putting API keys in plaintext for Hermes", May 11](https://www.reddit.com/r/hermesagent/comments/1tabisq/)

### Webhook Patterns

- **GitHub → Hermes:** Webhook receives PR events → Hermes reviews code, posts comments back via GitHub API.
- **Stripe → Hermes:** Payment events → agent logs transactions, sends alerts.
- **Custom webhooks:** Any service that can POST JSON can trigger Hermes. HMAC signature validation prevents spoofing.

### Cron + API = Automation

The most common automation pattern is cron job + API call:
1. Cron job fires on schedule
2. Hermes calls external API (weather, stocks, calendar, GitHub, etc.)
3. Agent processes data and posts summary to Telegram/Discord

### n8n Integration

n8n (visual workflow automation) + Hermes is a growing pattern:
- n8n handles the workflow graph and triggers
- Hermes provides the intelligence layer (decisions, natural language, tool selection)
- MCP bridges them

### Browser Automation

- **Browserbase web skill library:** A community member built a web skill library for advanced browser automation, gaining traction. [thread: "Have you tried this new web skill library by Browserbase?", Jun 10](https://www.reddit.com/r/hermesagent/comments/1u2fyuq/)
- **Built-in browser tools:** Hermes ships with browser_navigate, browser_click, browser_snapshot, browser_console, etc.

---

## Part 7: Authentication & Security for Integrations

### API Key Management — The Credential Problem

The "Stop putting API keys in plaintext" thread (May 11, 23 comments) surfaced the community's best thinking on this problem. Three main solutions emerged:

**OpenPass** (MIT license, community-built)
- CLI-first password manager with native MCP server
- Agent requests credentials via MCP → human approves (TouchID/Windows Hello) → session caches in OS keyring (15-min default)
- Uses `age` (X25519) encryption — no GPG complexity
- Git-synced vault, imports from 1Password/Bitwarden
- Trade-off: credentials do reach the agent (with permission), meaning they can appear in chat logs/model APIs

**Agent Vault** (Infisical, mixed license)
- Proxy-based credential brokering — agents NEVER touch raw credentials
- One command: `agent-vault run -- hermes` scaffolds everything
- Credentials injected at the proxy layer; agent only sees proxy tokens
- Trade-off: needs running server process, CA cert in every agent env, `ee` directory with premium enterprise features
- Community feedback: "agents get tripped up by the proxy" but improving rapidly

**UnifyKeys** (proxy token model)
- Store provider keys in encrypted vault, get a proxy token
- App only sees the proxy token — never the real key
- Usage tracking per API/provider, IP visibility, revoke/block suspicious traffic
- Free GitHub key scanner for exposed credentials

**Community consensus on credential security (ranked):**

1. **MCP OAuth** — Best option where supported. No secrets on disk. Supported for Linear, Sentry, Stripe, Atlassian, Asana, Figma.
2. **Agent Vault / credential brokering** — Best architectural guarantee. Agents never hold real credentials even in memory. Worth the setup complexity for production use.
3. **OpenPass / password manager + MCP** — Good balance. Secrets retrieved at runtime with human approval. Simpler than Agent Vault but credentials enter agent context.
4. **UnifyKeys proxy token** — Good for LLM provider keys specifically. Simpler than full MCP setup.
5. **`~/.hermes/.env` with restrictive permissions** — Acceptable minimum. `chmod 600`. Better than config.yaml but still plaintext on disk.
6. **Plaintext in config.yaml** — Avoid. Multiple threads warn against this.

**The core tension:** Convenience (secrets available when the agent needs them) vs exfiltration prevention (secrets never in agent memory, chat logs, or model APIs). Agent Vault represents the exfiltration-prevention extreme; OpenPass the convenience extreme. The right answer depends on your threat model.

**Fresh discussion — today (Jun 28, 2026):** A thread asking "Credential management: what's the state of the art on Hermes?" confirms this is an active concern. The OP specifically worried about agents retrieving credentials and passing them back via prompt injection. Community responses: Infisical/Agent Vault recommended as the credential-brokering solution; a new tool "taOS" with agent-specific access keys also mentioned. The consensus: credential brokering (agents never see real secrets) is the direction the community is heading. [thread: "Credential management: what's the state of the art on Hermes?", 6 upvotes, 5 comments, Jun 28](https://www.reddit.com/r/hermesagent/comments/1ui3137/)

### Sandbox Considerations

- **Docker:** Running Hermes in a container limits filesystem access but complicates tool access (file paths, local services).
- **VM isolation:** Some users run Hermes in a dedicated VM for complete separation. [thread: "Security & Paranoia & Fun", Jun 14](https://www.reddit.com/r/hermesagent/comments/1u5cqb6/)
- **VPS:** Cloud VPS provides network isolation from your home network. Still requires firewall hardening. [thread: "Is VPS option good for privacy & security?", Jun 12](https://www.reddit.com/r/hermesagent/comments/1u492zr/)

### Permission Prompts

Hermes's safety layer may flag and require confirmation for:
- Financial files (spreadsheets with expense data)
- Destructive operations (file deletion, directory removal)
- External API calls to new domains

This is configurable but defaults to safe. The community generally recommends keeping safety prompts enabled for integrations that touch sensitive data.

---

## Part 8: FAQ

1. **Which messaging platform should I start with?** Telegram. Best-documented, most community support, simplest setup.

2. **Can I use multiple messaging platforms at once?** Yes — the gateway handles routing. Same agent, same memory, different frontends.

3. **Will Google ban my Gmail if Hermes uses it?** Very likely for free Gmail accounts. Use Google Workspace (paid) or a dedicated email provider.

4. **Obsidian or Notion for my knowledge base?** Obsidian — local files, no API rate limits, better Hermes compatibility. Notion if you need collaboration features.

5. **How do I connect Hermes to Home Assistant?** Install the community HA add-on (5-minute setup). Use Hermes as the intelligence layer, HA for device control.

6. **What's the safest way to store API keys?** MCP OAuth where supported. Otherwise, a local password manager wrapped in an MCP server.

7. **How do I find quality skills in the 90K+ Skills Hub?** Check subreddit recommendations, sort by recent installs, review author reputation. Prefer skills updated in the last 3 months.

8. **Can Hermes create its own skills?** Yes — it can self-evolve skills based on experience. The skill-audit workflow (review → patch → test) refines them.

9. **What's the difference between a skill and an MCP server?** Skills are Hermes-specific (markdown instructions + optional scripts). MCP servers are external tool servers using the Model Context Protocol standard — language-agnostic and reusable across MCP-compatible clients.

10. **How do I trigger Hermes from external services?** Webhooks adapter receives POST requests, validates signatures, and routes to the agent. Cron jobs for scheduled triggers.

11. **Can Hermes browse the web automatically?** Yes — built-in browser tools (browser_navigate, browser_click, etc.) plus external libraries like Browserbase's web skill library.

12. **What's the best OAuth setup for cloud APIs?** Use MCP's built-in OAuth 2.1 support. For remote/VPS hosts, use the paste-back flow (copy redirect URL from browser).

---

## Part 9: Knowledge Table — Every Integration, Tool & Plugin

| Integration | Category | Type | Setup Difficulty | Free Tier | Best For | Watch For |
|------------|----------|------|-----------------|-----------|----------|-----------|
| Telegram Gateway | Messaging | Native | Easy | Free | Primary chat interface | Message formatting quirks |
| Discord Gateway | Messaging | Native | Easy | Free | Community bots | — |
| WhatsApp Gateway | Messaging | Native | Medium | Free | Mobile-first users | Setup more involved |
| Signal Gateway | Messaging | Native | Medium | Free | Privacy-focused | — |
| Email Gateway | Messaging | Native | Medium | Free (bring provider) | Async communication | Gmail bans bot activity |
| Himalaya CLI | Email | Skill | Easy | Free (OSS) | IMAP/SMTP agent email | Terminal-only |
| Obsidian Skill | Knowledge | Skill | Easy | Free (OSS) | Local knowledge base | Vault must be accessible |
| Notion API | Knowledge | Skill | Medium | Free (limited API) | Collaborative knowledge | API rate limits |
| Google Calendar | Productivity | Skill (gws CLI) | Medium | Free | Scheduling/reminders | Gmail account risk |
| Gmail (via gws) | Productivity | Skill (gws CLI) | Medium | Free (personal) | Agent email | Ban risk on free accounts |
| Apple Notes (memo) | Productivity | Skill (CLI) | Easy | Free (macOS) | Quick notes | Occasional skill issues |
| Apple Reminders | Productivity | Skill (CLI) | Easy | Free (macOS) | Task management | macOS-only |
| iMessage | Messaging | Photon Spectrum plugin | Easy | Free (Photon managed line) | SMS/iMessage, all platforms | New in v0.17.0 — no Mac required |
| SimpleX | Messaging | Bundled plugin | Easy | Free (OSS) | Privacy-first messaging | New in v0.17.0 |
| WhatsApp Business Cloud | Messaging | Native adapter | Easy | Paid (Meta) | Official WhatsApp API | New in v0.17.0 — no bridge process |
| Home Assistant | Home Automation | Community Add-on | Easy | Free (OSS) | Smart home control | Can't edit automations directly |
| MQTT | IoT | Protocol | Medium | Free | Sensor data | Requires broker setup |
| MCP Catalog | Developer | Native | Easy | Free | One-click tool installs | Read manifest carefully |
| GitHub MCP | Developer | MCP Server | Easy | Free | Code/issue management | PAT required |
| n8n | Automation | External + MCP | Medium | Free (self-hosted) | Visual workflows | Adds infrastructure |
| Webhooks Adapter | Developer | Native | Medium | Free | External triggers | Requires public endpoint |
| Browserbase Skill | Browser | External Skill | Medium | Free tier available | Advanced web automation | External dependency |
| xurl CLI | Social | Skill (CLI) | Medium | Free (X API) | X/Twitter integration | API access required |
| Shopify MCP | E-commerce | Community MCP | Medium | Paid (Shopify) | Store management | Community-maintained |
| Amazon MCP | E-commerce | Community MCP | Medium | Paid (AWS) | Product intelligence | Community-maintained |
| Bitwarden MCP | Security | Community MCP | Medium | Free (OSS) | Secret management | Setup involves local server |
| Todoist | Productivity | API | Medium | Free tier | Task management | API integration custom |
| Excel/Sheets | Productivity | Native | Easy | Free | Spreadsheet data | Safety prompts on financial data |

---

## Part 10: Sources & Contribute

This megathread is built from ~42 community threads, X/Twitter posts, and official Hermes docs from late April – June 28, 2026. Updated against v0.17.0 release notes (June 19, 2026). Key sources include:

- r/hermesagent subreddit discussions
- X/Twitter: @NousResearch, @WolframRvnwlf, community builders
- Official Hermes Agent docs (hermes-agent.nousresearch.com)
- GitHub: NousResearch/hermes-agent, Skills Hub

**Something missing? Wrong?** Reply with corrections, additions, or your own integration setup. Community megathreads improve through contribution.

**See also:**
- [Models, Providers & Plans Megathread](https://www.reddit.com/r/hermesagent/comments/1ufrtsf/) — for model selection and cloud provider comparisons
- [Multi-Agent & Profiles Megathread](https://www.reddit.com/r/hermesagent/comments/1ucmvi8/) — for running multiple agents and profiles
- [Kanban Setups Megathread](https://www.reddit.com/r/hermesagent/comments/1ugkihk/) — for task orchestration and Kanban boards
- [Cost & Token Optimization Megathread](https://www.reddit.com/r/hermesagent/comments/1ud03si/) — for keeping costs under control

---

## Comment-Sourced Updates

- **Matrix omission:** commenters flagged Matrix as missing from the messaging survey. Matrix is retained as a community-requested integration, not represented here as officially supported until current Hermes documentation confirms the support path.

- **Credential boundaries:** avoid reusing one provider credential across unrelated agents and services when scoped, revocable credentials are available. Third-party credential-broker products mentioned in comments remain unverified examples.

- **Plugin maturity:** local-knowledge, memory, Home Assistant, and enterprise-integration projects mentioned in comments must be checked for maintenance, compatibility, and disclosure before recommendation. Vendor promotional claims are archived rather than endorsed.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
