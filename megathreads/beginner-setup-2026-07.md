# How to Set Up Hermes Agent from Scratch (2026 Beginner Guide)

**LAST UPDATED: July 8, 2026**
**Covers: v0.18.0 ("The Judgment Release") + community threads from June–July 2026**

**GitHub mirror (permanent, Google-indexed):** [github.com/AtlasOmnia/hermesagent-megathreads](https://github.com/AtlasOmnia/hermesagent-megathreads/blob/main/megathreads/beginner-setup-2026-07.md)
**Also see:** [Best Free Models & APIs for Hermes Agent](https://www.reddit.com/r/hermesagent/comments/1uj9nkn/) · [Model Civil War — Local vs Cloud vs Hybrid](https://www.reddit.com/r/hermesagent/comments/1uqd00s/) · [Multi-Agent & Profiles Megathread](https://www.reddit.com/r/hermesagent/comments/1ucmvi8/) · [Self-Host Hermes on a VPS](https://www.reddit.com/r/hermesagent/comments/1tw9lbd/)

---

Want to set up Hermes Agent (by Nous Research) but don't know where to start? Hermes is an open-source, provider-agnostic AI agent that can use tools, run code, control your browser, and chat across multiple platforms — think of it as a personal AI assistant you control completely, not a corporate chatbot. This community-sourced guide — updated for v0.18.0 — gets you from zero to a working AI agent in under 20 minutes. It covers what the official docs don't tell you: the real first-time experience, the mistakes everyone makes, and the setup paths that actually work. Basic terminal familiarity helps, but no AI experience required.

---

## TL;DR — The 5-Minute Quick Start

| Decision | Community Pick | Why |
|----------|---------------|-----|
| **Install method** | Desktop installer (macOS/Windows) or curl one-liner (Linux/WSL) | One step, no manual dependency hunting |
| **First setup path** | `hermes setup --portal` (Nous Portal) | Signs you in, picks a model, starts chatting — no config needed |
| **First model for beginners** | Nous Portal free tier (auto-selected) | No API key, no payment, just works |
| **Local model path** | `hermes setup` → choose local/custom endpoint | For privacy, offline, or free local models |
| **Where to ask for help** | This subreddit (HELP flair) + [Official Discord](https://discord.gg/nousresearch) | Fastest response from community |
| **First thing to type** | `/help` (inside Hermes) or `hermes chat -q "What can you do?"` (in terminal) | Confirms everything works |
| **Stuck?** | `hermes doctor` (or `hermes doctor --fix`) | Diagnoses install, config, and model connectivity in one command |

**The single most important tip from the community:** Just install it and ask Hermes itself about your concerns. After setup, Hermes itself is often the fastest way to discover what it can do. [thread: "Initial setup for total newbie", Jun 18](https://www.reddit.com/r/hermesagent/comments/1u9f7xb/)

---

### Jargon Buster (Quick Reference)

| Term | Plain English |
|------|---------------|
| **LLM** | The AI model that powers Hermes (like an engine in a car) |
| **API key** | A password that lets Hermes talk to a cloud AI service |
| **VRAM** | Graphics card memory — needed if you run models locally on your GPU |
| **Provider** | The company or service supplying the AI model (OpenAI, DeepSeek, Google, etc.) |
| **Gateway** | The part of Hermes that runs 24/7 so you can chat from your phone |
| **Profile** | A completely separate Hermes identity with its own settings, memory, and skills |
| **Tool calling** | The model's ability to DO things (search web, run commands) rather than just chat |

---

## Part 1: Installation — The Two Paths

### Path A: Desktop Installer (Recommended for Beginners)

The easiest path. Download the installer from [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com/) and run it. This installs both the Desktop app and the `hermes` CLI command. After install, run `hermes setup --portal` for the zero-config quick setup.

If you skip the Desktop installer and do CLI-only, you can add the desktop app later with `hermes desktop`.

### Path B: Command-Line Only

    # Linux / macOS / WSL2 / Android (Termux)
    curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash

    # Windows (native PowerShell)
    iex (irm https://hermes-agent.nousresearch.com/install.ps1)

After install, reload your shell (`source ~/.zshrc` or `source ~/.bashrc`) and run:

    hermes setup --portal       # Quick Setup: zero-config, Nous Portal model
    # OR
    hermes setup                # Full wizard: pick model, tools, gateway, everything

`hermes setup --portal` signs you into Nous Portal, picks a working model, and drops you into chat. No config files, no API keys, no model selection stress. If you just want to see what Hermes can do, this is the path.

**Bonus: If you're on a Nous Portal paid plan,** the Tool Gateway automatically bundles web search, image generation, TTS, and cloud browser — no separate API keys needed. This is the single biggest v0.18.0 quality-of-life upgrade for beginners. [Docs: Tool Gateway](https://hermes-agent.nousresearch.com/docs/integrations/nous-portal)

### OS-Specific Notes

| Platform | Install Command | Watch For |
|----------|----------------|-----------|
| **macOS** | Desktop installer or curl one-liner | Works out of the box |
| **Linux** | curl one-liner | Install `uv` if missing; gateway needs `systemd --user` or `loginctl enable-linger` |
| **Windows (WSL2)** | curl one-liner inside WSL | Requires `systemd=true` in `/etc/wsl.conf` for gateway persistence |
| **Windows (native)** | Desktop installer or PowerShell one-liner | Works out of the box. Windows Terminal users: Alt+Enter is captured for fullscreen — use Ctrl+Enter for newlines |

**Community tip:** The gateway dies on WSL2 close without `systemd=true` — this is the #1 Windows support question. [thread: "How To Set Up Hermes (Desktop, Local + Cloud LLM, Profiles...)", Jun 30](https://www.reddit.com/r/hermesagent/comments/1ukkltg/)

---

## Part 2: Choosing Your First Model

This is where most beginners get stuck. Here's the decision tree:

    Do you want to pay for API access?
    ├── YES → Use Nous Portal (hermes setup --portal) — auto-selects a capable model
    │         OR: OpenRouter with any model you prefer (~$0-5/month)
    │
    └── NO → Do you have a GPU?
        ├── YES (8GB+ VRAM) → Run a local model via LM Studio or Ollama
        │   └── Recommended: Qwen2.5-7B or Gemma 3 12B (good tool-calling)
        │
        ├── YES but <8GB VRAM → Treat as "no GPU" for Hermes purposes — use free cloud tier
        │
        └── NO → Use a free cloud API tier
            └── Google Gemini (free tier) or DeepSeek (dirt-cheap API)

### Community Consensus on Beginner Models

| Model | Type | Cost | Tool-Calling | Verdict |
|-------|------|------|-------------|---------|
| **Nous Portal default** | Cloud | Free to start | Excellent | Best zero-config option |
| **DeepSeek V3** | Cloud API | ~$0.27/M input, ~$1.10/M output | Very good | Best value cloud model |
| **Gemini 2.5 Flash** | Cloud API | Free tier | Good | Best free cloud option |
| **Qwen2.5-7B** | Local | Free | Passable | Runs on 8GB VRAM; tool-calling is borderline at 7B — upgrade to 12B+ when possible |
| **Gemma 3 12B** | Local | Free | Decent | Needs 12GB+ VRAM; CPU-only is impractically slow |
| **Llama 3 8B** | Local | Free | Weak | Struggles with tool reliability; not recommended for agentic use |

**Critical warning from the community:** 8B models (Llama 3 8B, Gemma 3 8B) are NOT enough for reliable tool use. Hermes needs at least a 12B model for consistent tool-calling. If your agent ignores tools or hallucinates commands, your model is too small. [thread: "why can not my hermes agent use tools?", Apr 21](https://www.reddit.com/r/hermesagent/comments/1srpkrz/)

**Model switching:** Change models anytime with `hermes model` (interactive picker) or `/model <name>` in-session. The fuzzy picker in v0.18.0 lets you type "v4fl" and get `deepseek-v4-flash`. [thread: "Help me understand how to use agents and models", May 10](https://www.reddit.com/r/hermesagent/comments/1t910oz/)

For a deeper dive into model options, see the [Best Free Models & APIs Megathread](https://www.reddit.com/r/hermesagent/comments/1uj9nkn/). For the local vs cloud vs hybrid debate: [Model Civil War Megathread](https://www.reddit.com/r/hermesagent/comments/1uqd00s/).

---

## Part 3: Essential Tools — What to Enable First

Hermes ships with many tools. You don't need all of them on day one. Here's what the community recommends enabling first:

### Day 1 Tools

    hermes tools enable web        # Web search and content extraction
    hermes tools enable file       # Read/write files on your system
    hermes tools enable terminal   # Run shell commands
    hermes tools enable memory     # Remember preferences across sessions

### Why These Four?

- **Web** — lets Hermes search the internet and extract page content. Most "why can't Hermes help me?" issues are because web search isn't enabled.
- **File** — read/write/search files. Essential for any productivity task.
- **Terminal** — run commands. Hermes is an agent, not a chatbot — this is where it gets useful.
- **Memory** — persistent cross-session memory. Without it, Hermes forgets everything between sessions.

### The #1 Beginner Trap: "Tools Not Available"

**Dozens of beginners hit this:** They install Hermes, start chatting, and the agent can't use any tools. The fix: `/tools` shows no tools? That's because tools are **disabled by default** on some platforms. Run `hermes tools` to open the interactive picker — it's scoped per-platform, so enable tools for each platform you use (CLI, Desktop, Gateway). Then `/reset` to start a fresh session. [thread: "For anybody hitting the /tools shows no tools available", May 25](https://www.reddit.com/r/hermesagent/comments/1tni92q/)

### Tool Changes Require a Fresh Session

This is the second-most common frustration: you enable tools, they still don't work. Tools are loaded at session start. After any tool enable/disable, run `/reset` or start a new `hermes` session. The old session keeps the old toolset.

---

## Part 4: Common First-Time Pitfalls

### Pitfall #1: The "Fetus in Fetu" Install (Nested/Double Installation)

**Symptom:** You installed Hermes, then installed it again from a different method, and now you have TWO Hermes directories fighting each other. Docker containers get corrupted, configs conflict, nothing works.

**Fix:** Uninstall completely (`hermes uninstall`), remove `~/.hermes/`, and reinstall once. Avoid running two installations concurrently. If you switch methods, cleanly uninstall the old one first. [thread: "I accidentally created a fetus in fetu Hermes Agent installation", Jun 24](https://www.reddit.com/r/hermesagent/comments/1uer0xy/)

### Pitfall #2: "Model Not Working" / Free Model Frustration

**The reality:** Hermes Agent is free and open-source. The models it connects to may not be. Free models often have weak tool-calling, small context windows, or rate limits that break agent workflows. This sounds harsh, but the fix is simple — several free or dirt-cheap options actually work well. [thread: "Why is Hermes not working with any free model?", May 25](https://www.reddit.com/r/hermesagent/comments/1tn6t8i/)

**The fix:** Use Nous Portal's free tier, Google Gemini's free tier, or DeepSeek's cheap API (~$0.27/M input tokens — a few dollars goes a long way). Or run a local 12B+ model.

### Pitfall #3: Agent Doesn't Actually DO Anything

**Symptom:** The agent responds but doesn't actually DO anything useful. It's chatty but not agentic.

**Common causes:**
- Model too small (8B class models fail at tool calling)
- Tools not enabled (see "The #1 Beginner Trap" in Part 3)
- Using a model that has poor instruction-following for tool use (some instruct-tuned models resist multi-step tool calling)
- Context window too small for the task

**Fix:** Switch to a model known for good tool-calling (DeepSeek V3, Claude, Qwen2.5-14B+). Run `hermes model` to pick a different one. [thread: "Hermes agent not working as expected!", May 18](https://www.reddit.com/r/hermesagent/comments/1th1gig/)

### Pitfall #4: "Model Not Connecting"

**Symptom:** `hermes` starts but can't connect to the model. Error messages about API keys or connection refused.

**Fix:** Run `hermes doctor --fix` — it checks config, dependencies, and model connectivity, and auto-repairs common issues. Then `hermes model` to reconfigure. If using a local model, verify the server is running (LM Studio/Ollama). If using a cloud model, verify your API key in `~/.hermes/.env`. [thread: "Not able to connect the model", Apr 15](https://www.reddit.com/r/hermesagent/comments/1smhvk2/)

### Pitfall #5: "Config Changes Not Taking Effect"

**Config changes** require a gateway restart (`hermes gateway restart`) or CLI exit/relaunch. **Tool changes** require `/reset`. **Model changes** via `/model` are immediate but only for the current session. **Persistent model changes** require `hermes model` or `hermes config set model.default <model>`.

---

## Part 5: Profiles — What They Are and When to Use Them

Profiles are the #1 feature beginners don't discover until they've already made a mess. Here's what you need to know:

### What Profiles Do

A profile is a completely separate Hermes instance with its own:
- Config (different model, tools, personality)
- Memory (doesn't mix work and personal conversations)
- Skills (coding agent doesn't load your recipe skills)
- Sessions (separate chat history)

### When to Create a Profile

| Scenario | Profile Setup |
|----------|--------------|
| Work + Personal | `hermes profile create work` and `hermes profile create personal` |
| Coding + General | `hermes profile create dev` with coding tools only |
| Multi-model testing | `hermes profile create test` to try different models safely |
| Dedicated task agent | `hermes profile create <name> --clone-from default` |

### The Community's Profile Philosophy

The community is split on profiles. The "minimalists" run one profile for everything. The "specialists" create profiles per use case. The consensus: **start with one profile, learn the tool, then split when you notice context pollution** (work memories showing up in personal chats, or vice versa). [thread: "My simplest yet effective hermes agent profile setup", May 7](https://www.reddit.com/r/hermesagent/comments/1t66lhy/)

For a comprehensive profile strategy guide, see the [Multi-Agent & Profiles Megathread](https://www.reddit.com/r/hermesagent/comments/1ucmvi8/).

**Key commands:**

    hermes profile create <name>     # New profile
    hermes --profile <name>          # Use a profile once
    hermes profile use <name>        # Set as default
    hermes profile list              # See all profiles
    <name> chat                      # v0.18.0: every profile gets its own shell command

---

## Part 6: Desktop App vs CLI — Which Interface?

Hermes v0.18.0 ships with three interfaces:

| Interface | Best For | How to Launch |
|-----------|----------|---------------|
| **Desktop App** | Visual learners, multi-profile, coding projects | Download from hermes-agent.nousresearch.com (macOS/Windows) |
| **CLI** | Terminal natives, scripting, remote/SSH | `hermes chat` (default) |
| **TUI** | Rich terminal UI with pickers, session browser | `hermes --tui` or configure as default |

### Desktop App (v0.18.0)

The desktop app includes:
- Per-profile coding Projects sidebar
- Memory graph (visual timeline of everything Hermes knows about you)
- Remote-gateway connect (use your desktop on a different machine's Hermes)
- Multi-profile concurrent sessions
- `/learn` — turn anything into a reusable skill (highlight some text → `/learn`)
- `/journey` — playable timeline of accumulated memories and skills
- `/goal` — set a standing goal Hermes works toward across turns

---

## Part 7: Running Hermes 24/7 (Gateway Setup)

Want Hermes on your phone via Telegram, Discord, or WhatsApp? Set up the gateway:

    hermes gateway setup        # Pick your platforms
    hermes gateway install      # Install as background service
    hermes gateway start        # Start the service

**Persistence tips:**
- **Linux:** `sudo loginctl enable-linger $USER` (prevents death on logout)
- **macOS:** `hermes gateway install` creates a launchd plist — survives reboots
- **WSL2:** Set `systemd=true` in `/etc/wsl.conf` or the gateway dies on WSL close
- **VPS:** See the [Self-Host Hermes on a VPS Megathread](https://www.reddit.com/r/hermesagent/comments/1tw9lbd/) for full deployment walkthrough

---

## Part 8: FAQ — Real Questions from r/hermesagent

1. **Q: I installed Hermes. Now what?**
   **A:** Type `hermes` and start chatting. Or run `hermes setup --portal` for the guided Quick Setup with a working model.

2. **Q: Do I need to pay for anything?**
   **A:** Hermes Agent is free and open-source (MIT). You only pay for cloud model API access if you choose a paid model. Free options exist: Nous Portal free tier, Google Gemini free tier, or local models. See the [Free Models & APIs Megathread](https://www.reddit.com/r/hermesagent/comments/1uj9nkn/).

3. **Q: What's the best free model to start with?**
   **A:** Google Gemini 2.5 Flash (free tier, good tool-calling) or DeepSeek V3 (dirt-cheap, excellent tool-calling). For local: Qwen2.5-7B or Gemma 3 12B.

4. **Q: Why won't my agent use tools?**
   **A:** Three things to check: (1) tools enabled via `hermes tools`, (2) `/reset` after enabling, (3) model is at least 12B — 8B models fail at tool calling.

5. **Q: How do I change models?**
   **A:** `hermes model` (interactive) or `/model <name>` in-session. The fuzzy picker makes this fast.

6. **Q: Can I run Hermes on my phone?**
   **A:** Yes — set up the gateway with Telegram, Discord, or WhatsApp, then chat from your phone. The Desktop App can also connect to a remote gateway.

7. **Q: What's the difference between Hermes and Claude Code / Cursor?**
   **A:** Hermes is provider-agnostic (any model), has persistent memory, runs on messaging platforms, and is fully open-source. Claude Code is Anthropic-only. Cursor is an IDE, not an agent. [thread: "Hermes vs. Claude Code (Remote Control)", Jun 30](https://www.reddit.com/r/hermesagent/comments/1ujx3kr/)

8. **Q: How do I keep Hermes running 24/7?**
   **A:** `hermes gateway install` (systemd on Linux, launchd on macOS). Enable linger: `sudo loginctl enable-linger $USER`. For a full VPS setup, see the [VPS Megathread](https://www.reddit.com/r/hermesagent/comments/1tw9lbd/).

9. **Q: My config changes don't work. What's wrong?**
   **A:** Some changes need a restart: `hermes gateway restart` (gateway), exit/relaunch (CLI), `/reset` (tools). Model changes via `/model` are immediate but session-only; persistent changes need `hermes config set model.default <model>`.

10. **Q: How do I learn what Hermes can do?**
    **A:** Type `/help` in any session. Ask Hermes directly: "What tools do you have? What can you help me with?" Run `hermes --help` for CLI commands. Run `hermes doctor` for health checks.

11. **Q: How do I fix a broken install?**
    **A:** Run `hermes doctor --fix` first (auto-repairs common issues). If still broken: `hermes uninstall`, remove `~/.hermes/`, reinstall clean.

12. **Q: Can I use Hermes with my local LLM?**
    **A:** Yes. Run LM Studio or Ollama, then `hermes setup` → choose custom/local endpoint → point it at your server's URL (e.g., `http://localhost:1234/v1`).

---

## Part 9: Sources & Threads

### Community Threads (r/hermesagent)

- [How To Set Up Hermes (Desktop, Local + Cloud LLM, Profiles...), Jun 30, 2026](https://www.reddit.com/r/hermesagent/comments/1ukkltg/)
- [Initial setup for total newbie, Jun 18, 2026](https://www.reddit.com/r/hermesagent/comments/1u9f7xb/)
- [First time Hermes user, looking for initial setup advice, Jun 18, 2026](https://www.reddit.com/r/hermesagent/comments/1u901km/)
- [I accidentally created a fetus in fetu Hermes Agent installation, Jun 24, 2026](https://www.reddit.com/r/hermesagent/comments/1uer0xy/)
- [Complete Hermes Agent Setup Guide, Mar 14, 2026](https://www.reddit.com/r/hermesagent/comments/1rt5syt/)
- [Local LLM Beginner Setup Guide for Hermes Agent, Apr 22, 2026](https://www.reddit.com/r/hermesagent/comments/1sslax1/)
- [For anybody hitting the /tools shows no tools available, May 25, 2026](https://www.reddit.com/r/hermesagent/comments/1tni92q/)
- [why can not my hermes agent use tools?, Apr 21, 2026](https://www.reddit.com/r/hermesagent/comments/1srpkrz/)
- [WHY TF is Hermes not working with any free model!!, May 25, 2026](https://www.reddit.com/r/hermesagent/comments/1tn6t8i/)
- [Hermes agent not working as expected!, May 18, 2026](https://www.reddit.com/r/hermesagent/comments/1th1gig/)
- [Not able to connect the model, Apr 15, 2026](https://www.reddit.com/r/hermesagent/comments/1smhvk2/)
- [Help me understand how to use agents and models, May 10, 2026](https://www.reddit.com/r/hermesagent/comments/1t910oz/)
- [My simplest yet effective hermes agent profile setup, May 7, 2026](https://www.reddit.com/r/hermesagent/comments/1t66lhy/)
- [Hermes vs. Claude Code (Remote Control), Jun 30, 2026](https://www.reddit.com/r/hermesagent/comments/1ujx3kr/)
- [Hermes Agent with Open Source LLM Setup help, Jun 8, 2026](https://www.reddit.com/r/hermesagent/comments/1u0djpe/)
- [Best Free Models & APIs Megathread, Jun 2026](https://www.reddit.com/r/hermesagent/comments/1uj9nkn/)
- [Multi-Agent & Profiles Megathread, Jun 22, 2026](https://www.reddit.com/r/hermesagent/comments/1ucmvi8/)
- [Model Civil War Megathread, Jul 2026](https://www.reddit.com/r/hermesagent/comments/1uqd00s/)
- [Self-Host Hermes on a VPS Megathread, May 2026](https://www.reddit.com/r/hermesagent/comments/1tw9lbd/)

### Official Sources

- [Hermes Agent v0.18.0 Release Notes](https://github.com/NousResearch/hermes-agent/releases/tag/v2026.7.1) — July 1, 2026
- [Hermes Agent Installation Docs](https://hermes-agent.nousresearch.com/docs/getting-started/installation/)
- [Hermes Agent Configuration Docs](https://hermes-agent.nousresearch.com/docs/user-guide/configuration/)
- [Hermes Agent Profiles Docs](https://hermes-agent.nousresearch.com/docs/user-guide/profiles/)
- [Hermes Agent Messaging Docs](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/)

### External Guides

- [LumaDock: Hermes Agent Complete Self-Hosting Guide](https://lumadock.com/tutorials/hermes-agent-complete-guide) — Jun 17, 2026
- [YouTube: Hermes Agent + Ollama Local Install](https://www.youtube.com/watch?v=aAQWB3-XD5s) — Apr 23, 2026

---

## Part 10: Knowledge Table — Every Setup-Related Tool & Concept

| Category | Item | Description | Best For | Watch For |
|----------|------|-------------|----------|-----------|
| **Install** | Desktop installer | One-click macOS/Windows install | Beginners | Recommended over CLI-only |
| **Install** | curl one-liner | One-command Linux/macOS/WSL install | Most users | Don't mix with git clone or Docker |
| **Install** | PowerShell install | Windows native install | Windows without WSL | Both paths are well-supported now |
| **Install** | Docker install | Containerized Hermes | Servers, CI/CD pipelines | Not for daily desktop use |
| **Setup** | `hermes setup --portal` | Quick Setup via Nous Portal | Beginners, zero-config | Requires internet |
| **Setup** | `hermes setup` | Full interactive wizard | Power users, local models | Can be overwhelming |
| **Model** | Nous Portal | Managed cloud models | Beginners | Free tier available |
| **Model** | DeepSeek V3 | Cloud API, cheap | Best value cloud | Text-only (no vision) |
| **Model** | Gemini 2.5 Flash | Google cloud, free tier | Best free cloud | Rate limits apply |
| **Model** | Local LLM (LM Studio) | Local inference on your GPU | Privacy, offline | Needs 8GB+ VRAM minimum |
| **Model** | Local LLM (Ollama) | Local inference, easy setup | Quick local testing | Slower than LM Studio |
| **Model** | OpenRouter | Multi-provider API gateway | Model comparison, one API key | Adds latency |
| **Interface** | Desktop App | Visual app with Projects | Multi-profile, visual users | macOS/Windows (Linux uses CLI) |
| **Interface** | CLI | Terminal-based chat | SSH, scripting, speed | No visual pickers |
| **Gateway** | Telegram/Discord/WhatsApp | Chat from your phone | Mobile access | Gateway must stay running |
| **Tool** | Web search | Internet search + extraction | Research, current events | Enable early |
| **Tool** | Terminal | Run shell commands | Automation, coding | Approval prompts by default |
| **Tool** | File | Read/write files | Productivity | Respects filesystem permissions |
| **Tool** | Memory | Cross-session recall | Long-term use | Enable day one |
| **Config** | `hermes doctor --fix` | Health check + auto-repair | Troubleshooting | Run first when stuck |
| **Config** | `hermes config check` | Config validation | After upgrades | New in v0.18.0 |
| **Config** | `hermes config migrate` | Update config for new options | After upgrades | Run after `hermes update` |
| **Profile** | `hermes profile create` | New isolated instance | Work/personal split | Start with one profile |

---

## Related Megathreads (Internal Linking)

| Megathread | Best If You Want To... |
|-----------|------------------------|
| [Best Free Models & APIs](https://www.reddit.com/r/hermesagent/comments/1uj9nkn/) | Find free/cheap models that actually work |
| [Model Civil War: Local vs Cloud vs Hybrid](https://www.reddit.com/r/hermesagent/comments/1uqd00s/) | Decide between local, cloud, or hybrid deployment |
| [Self-Host Hermes on a VPS](https://www.reddit.com/r/hermesagent/comments/1tw9lbd/) | Run Hermes 24/7 on a cheap cloud server |
| [Multi-Agent & Profiles](https://www.reddit.com/r/hermesagent/comments/1ucmvi8/) | Set up work/personal splits and multi-agent workflows |
| [Top Posts of All Time](https://www.reddit.com/r/hermesagent/comments/1uqg0yz/) | See what the community's best work looks like |

---

**Got a setup tip or ran into a pitfall not covered here?** Drop it in the comments. This is a living document — corrections and additions welcome.

**GitHub mirror for permanent indexing:** [github.com/AtlasOmnia/hermesagent-megathreads](https://github.com/AtlasOmnia/hermesagent-megathreads/blob/main/megathreads/beginner-setup-2026-07.md) — open an issue or PR to suggest updates.

*Last refreshed: July 8, 2026.*

---

## Part 11: You're Set Up — Now What?

Here's what to do in your first week with Hermes:

| Day | Try This | What It Teaches You |
|-----|----------|---------------------|
| **Day 1** | Run `/help` and ask Hermes about itself | Discover what tools and features are available |
| **Day 2** | Ask Hermes to do a real task: "Search the web for X and summarize the results" | See agentic tool use in action |
| **Day 3** | Explore the Desktop App (if installed): memory graph, `/journey`, `/learn` | Understand what Hermes has learned about you |
| **Day 4** | Set up the gateway: `hermes gateway setup` → pick Telegram or Discord | Chat with Hermes from your phone |
| **Day 5** | Create a profile: `hermes profile create <name>` for a separate use case | Keep work and personal contexts isolated |
| **Week 2** | Browse the [Skills Hub](https://hermes-agent.nousresearch.com/docs/reference/skills-catalog/) | Extend Hermes with community-built capabilities |

Join the [Official Discord](https://discord.gg/nousresearch) — the fastest place to get help when you're stuck.
