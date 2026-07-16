# Multi-Agent & Profiles Megathread — Hermes Agent (June 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

**LAST UPDATED:** June 21, 2026
**Sourced from:** 14+ r/hermesagent threads, official Nous docs (profiles, delegation, Kanban, Swarm), GitHub source, community plugins.

# TL;DR — How Do I Run Multiple Agents?

|Decision|Community Pick|Why|
|:-|:-|:-|
|Separate work & personal|**Hermes profiles**|Isolated config, memory, sessions, skills — one machine, many agents|
|Inter-agent communication|**Kanban board** (shared task queue)|Durable, survives restarts, any agent can read/write|
|One bot, multiple agents|**Telegram topics** (one bot token → multiple profiles via topic routing)|Simpler than multiple bots|
|Multiple bot tokens|**One bot per profile**|Cleaner isolation if you have the tokens|
|Subagent delegation|**delegate\_task** for parallel research, code review, multi-file|Not for durable work — use Kanban for that|
|Agent teams managing themselves|**Kanban + profiles + cron**|Dispatcher spawns workers, they claim tasks, comment to coordinate|

# Part 1: Profiles — The Foundation

Profiles are the primary multi-agent primitive in Hermes. Each profile is a separate Hermes home directory with its own `config.yaml`, `.env`, `SOUL.md`, memories, sessions, skills, cron jobs, and gateway state.

# Quick Start

    # Create a coding agent
    hermes profile create coder
    coder setup          # configure API keys, model
    coder chat           # start chatting

    # Create a research agent cloned from your current config
    hermes profile create researcher --clone

    # Create a full clone (everything: config, memories, skills, cron)
    hermes profile create backup --clone-all

# What profiles actually isolate

|Isolated|Shared|
|:-|:-|
|config.yaml|Hermes installation (single codebase)|
|.env (API keys, tokens)|Python venv|
|SOUL.md (personality)|System tools|
|Sessions & conversations|Node.js / npm packages|
|Memory store|Playwright cache|
|Skills|GPU / hardware|
|Cron jobs||
|Gateway state (bot token, PID)||

# Command aliases

Every profile gets auto-generated commands at `~/.local/bin/<name>`:

    coder chat        # = hermes -p coder chat
    coder gateway start
    coder skills list
    coder config set model.default anthropic/claude-sonnet-4

# Different bot tokens per profile

Edit each profile's `.env` with your preferred text editor (for example `nano` on Linux/macOS or Notepad/PowerShell tooling on Windows):

    ~/.hermes/profiles/coder/.env
    ~/.hermes/profiles/default/.env

**Safety:** If two profiles accidentally use the same bot token, the second gateway is blocked with a clear error naming the conflicting profile.

# Profiles ≠ Sandboxes

Important: Profiles do NOT sandbox the agent on host installs. The agent still has your user's filesystem access. Profiles isolate Hermes state, not OS access.

If you need separate CLI identities per profile:

    # In the profile's config.yaml
    terminal:
      home_mode: profile   # Hermes uses {HERMES_HOME}/home as $HOME

Then initialize per-profile `~/.ssh`, `~/.gitconfig`, `gh` auth, etc. inside that profile's home.

# Part 2: Communication Patterns — How Agents Talk to Each Other

# 🥇 Pattern 1: Kanban Board (Shared Task Queue)

The official multi-agent coordination primitive. A durable SQLite-backed task board shared across all profiles.

    # Create a task for the coder profile
    hermes kanban create "Fix login timeout bug in auth.py" --assignee coder

    # Agent picks it up, works, updates status
    hermes kanban show <task-id>    # see comments, status, workspace

    # Human intervention
    hermes kanban comment <task-id> "Check the edge case at line 47"
    hermes kanban block <task-id> --reason "Waiting for staging deploy"
    hermes kanban unblock <task-id>

**Why Kanban beats delegate\_task for durable work:**

|Criterion|delegate\_task|Kanban|
|:-|:-|:-|
|Durability|Lost if parent is interrupted|Durable; survives restarts|
|Human in loop|Not supported mid-task|Supported via comments, block/unblock, reassignment|
|Multiple agents per task|One subagent per delegated job|Multiple profiles can claim, hand off, and comment over time|
|Audit trail|Easy to lose after compression/session churn|Comments/status/workspace history stay on the board|
|Resumability|None — failed = failed|Reclaim stale claims, unblock, reassign, continue|

**Kanban + profiles = a team:**

* Research agent reads docs, writes findings as comments
* Coder agent picks up the task, implements based on findings
* Review agent claims next, runs tests, comments review notes
* Human approves or sends back with a comment

# 🥈 Pattern 2: delegate_task (Parallel Research & Code Review)

For short, reasoning-heavy tasks where the parent needs results immediately.

    delegate_task(tasks=[
        {
            "goal": "Research WebAssembly outside the browser",
            "context": "Focus on: runtimes (Wasmtime, Wasmer), cloud/edge use cases",
            "toolsets": ["web"]
        },
        {
            "goal": "Research RISC-V server chip adoption",
            "context": "Focus on: shipping server chips, cloud providers adopting",
            "toolsets": ["web"]
        }
    ])

**What delegate\_task can't do:** survive restarts, involve humans mid-task, coordinate between agents over time. For those, use Kanban.

# 🥉 Pattern 3: Telegram Topics (One Bot, Multiple Profiles)

Route different Telegram topics to different Hermes profiles using a single bot token:

* Topic 1 (Personal) → default profile
* Topic 2 (Work) → work profile
* Topic 3 (Coding) → coder profile

Enable topic creation in BotFather, then configure topic routing in Hermes gateway.

Community note: "Telegram with threads on DM. Works like a charm. Just need to enable thread creation on BotFather and you're good to go. Just send /topic and..." — from messaging megathread

# Pattern 4: The Constitution Pattern (Community-Invented)

From u/codexshaper and others: Write a "constitution document" that defines each profile's specialty, capabilities, and routing rules. The orchestrator agent reads this document and routes work accordingly.

    # constitution.md
    ## Agent: coder
    - Specialty: Python, JavaScript, system scripts
    - Tools: terminal, file, github
    - Route: any coding, debugging, or implementation task

    ## Agent: researcher
    - Specialty: web research, docs reading, API exploration
    - Tools: web, file
    - Route: any research, documentation, or discovery task

    ## Agent: reviewer
    - Specialty: code review, security audit, test coverage
    - Tools: terminal, file, github
    - Route: any review, audit, or quality task

Then in your orchestrator's SOUL.md: "Read constitution.md before routing any task. Delegate to the profile whose specialty matches."

# Pattern 5: Multi-Agent Context Plugin (Community-Built)

From u/kaishi00: A Hermes plugin that injects chat history into each agent's context during their turn so they know what's happening in a shared channel. Supports Telegram.

    # Install on each profile:
    git clone https://github.com/kaishi00/hermes-community-plugins

**Critical config:** "make sure it require mentions" — without this, agents will talk to each other endlessly. Multiple users reported waking up to agents that had been conversing all night.

**Gateway config requirement:** On Discord, bots ignore mentions from other bots by default. You must update the gateway config to allow bot-to-bot mentions. On Telegram, bots need admin access in group chats (this is a Telegram limitation, not Hermes-specific — non-admin bots can only respond to slash commands or ping mentions).

# Pattern 6: Shared Memory / Context Bus

Advanced patterns from community builds:

* **A2A Context Bus** (u/ihgrant): A message-passing system where agents publish context updates to a shared bus, other agents subscribe and pull relevant context. Built as a Hermes plugin/skill.
* **Shared Kanban board with comments**: Agents use Kanban comments as an inter-agent protocol — each comment is visible to the next agent that picks up the task.
* **Honcho memory + profiles**: When Honcho is enabled, profile clones automatically create dedicated AI peers sharing the same user workspace while building independent observations.
* **NAS Shared Log** (u/Fun_Firefighter_7785): For agents on different physical machines. Uses append-only `.log` files with `fcntl.flock()` advisory locking over CIFS/NFS. Mount with `actimeo=0` for real-time sync. Critical finding: SQLite over network shares silently fails — file locks are advisory only. The text-log approach with proper locking is the reliable path.
* **agentsocket.dev**: A WebSocket-based agent communication service. Create a group chat via agent-socket, share the connect link with other agents.

# Pattern 7: Direct Agent-to-Agent via Telegram/Discord

Create a dedicated Telegram group or Discord channel where agents post to each other. Each agent has its own bot token in the group. Agents @mention each other, read messages, and respond.

**Critical gateway configs:**

* Discord: Bots ignore other bot mentions by default — update gateway config to allow bot-to-bot mentions
* Telegram: Bots need admin access for full functionality; non-admin bots can only respond to slash commands or ping mentions

Community caution from multiple users: This pattern can lead to agent loops and token waste. "I woke up one morning to two of my agents complaining all night about how they hadn't heard from me in several hours" — u/thetomsays (+26). Use mention-required mode and circuit breakers.

# Part 3: The Community's Real Setups

# The "Orchestrator + Specialists" Org Chart

The most common pattern across threads:

                     ┌─────────────┐
                     │ Orchestrator │  (default profile)
                     │  (FRIDAY)    │  Reads constitution, routes tasks
                     └──────┬──────┘
              ┌─────────────┼─────────────┐
              │             │             │
        ┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐
        │   Coder   │ │ Researcher│ │  Personal │
        │  profile  │ │  profile  │ │  profile  │
        └───────────┘ └───────────┘ └───────────┘

Each specialist has:

* Its own SOUL.md defining its role
* Restricted toolset (coder gets terminal+file+github, researcher gets web+file)
* Its own gateway (Telegram topic or separate bot)
* Tasks arrive via Kanban board or direct delegation

# The 4-Tier Scaling Model

From u/nemanja87mn (+20) — the community's definitive scaling framework:

|Tier|Setup|When to Use|
|:-|:-|:-|
|**Tier 1**|One agent, one gateway, delegate\_task for subagents|Protecting context without adding complexity|
|**Tier 2**|Orchestrator + specialist profiles, one chat surface|Multiple roles, isolated memory, one Telegram/Discord|
|**Tier 3**|Domain orchestrators with separate bots|Different businesses/domains (investment, coding, family)|
|**Tier 4**|Separate instances (same machine or VPS)|Client work, production, strict isolation|

**Key insight:** Most people never need Tier 3 or 4. "Start with a single agent and create as needed" — u/laffytaffykidd (+12).

# The Constitution Pattern (Community-Invented)

From u/Starrwulfe (+3) — collapse multiple profiles into one agent with split personalities:

>"Chances are those five agents can be one agent in Hermes that has five split personalities. Those split personalities can then be sub delegated to without you even having to do anything, even set up a profile."

How it works:

1. Create skill files in separate folders under the Hermes home directory
2. Each skill defines one "personality" — its specialty, tools, tone
3. Draft a `constitution.md` that maps specialties to personalities
4. The orchestrator reads the constitution before routing any task
5. delegate\_task launches the appropriate personality as a subagent

**Result:** "I went from having three separate agents and four profiles to having one agent with eight personalities and it's way more efficient." Profiles are powerful but sometimes overkill — a well-structured single agent with constitution-based routing can handle multiple domains cleanly.

# The Gradual Approach

From u/selipso (+3) — don't build the org chart on day one:

>**Week 1:** One gateway, two profiles (default + one specialized). Turn off all skills on the specialized profile except the ones you need (\~15-20 skills). Set up memory layer. Chat with it. Don't add tools.

>**Week 2+:** If context pollution occurs, add another profile. "Hermes takes about a week to fully settle into its role."

# The "Agent That Sets Up Agents" Meta-Pattern

From u/MrOzanges (+1): Ask your main Hermes agent to create a profile specifically for setting up other profiles. That specialist agent reviews SOUL.md, disables irrelevant tools/skills, and configures new profiles optimally. Meta: your agent architects your agent team.

# The Swarm Master Pattern

From u/mitchells00 (+1): A Hermes gateway whose job is commissioning new Hermes gateways — each a multi-profile swarm in its own Docker container. Limitation noted: "I'm hitting limits with how you cannot specify which agent profiles are relevant for which kanban boards."

# Hermes Swarm (Official)

A built-in feature for simultaneous multi-model queries (similar to Grok's approach). Multiple models process the same query in parallel; results are combined into a hybrid answer. Less documented than Kanban/delegation but exists as a distinct feature. Community awareness is low — when mentioned, most users haven't tried it.

# Real community setups:

**"I gave my Hermes team a shared Kanban board. Now they manage themselves."** — u/Ok_Run_5401

* Created a third-party Kanban board project (`pip install hermes-kanban`, github.com/amirghm/hermes-kanban) with auto-discovery of Hermes profiles
* Agents auto-appear from profiles — reads SOUL.md for name/description
* Includes an Agent-to-Agent (A2A) protocol for direct inter-agent messages
* Community response was split: many noted Hermes already has a **built-in native Kanban** (accessible via web dashboard and desktop app), while others appreciated the Telegram integration and simpler UI
* OP's take: "I know, but I designed a simpler one that suits my needs"
* **Recommendation:** Start with Hermes' built-in Kanban (zero setup, integrated with profiles and the dispatcher). Consider third-party tools only if you need specific features the native board doesn't offer (e.g., Telegram-native task management, custom A2A protocols).
* **Note:** This post showed signs of self-promotion — the OP's account had multiple recent posts promoting repos. Evaluate project links critically. The built-in Kanban is the supported path.

**"CrewAI/AutoGen aren't cutting it" thread resolution:**
The OP moved from CrewAI/AutoGen to Hermes profiles because:

* Profiles give persistent memory per agent (CrewAI agents lose context between runs)
* Kanban provides durable coordination (AutoGen's in-process handoffs are fragile)
* One machine runs all agents (CrewAI/AutoGen expect separate deployments)
* Community solution: delegate\_task + Engram shared memory + OpenClaw for execution: *"I'm just using a skill with --delegate-task from Hermes to openclaw and a shared memory through Engram system... Nothing is installed apart from that, all built using Hermes."* — u/djenttleman

**"Best Way to Separate Personal from Work and Telegram"** — u/harpr1t

* Default profile = personal agent (one Telegram bot)
* Work profile = work agent (separate Telegram bot)
* Docker on NAS running both
* Key learning: two bots = cleanest separation. Topics work but bots are simpler for strict separation.

# Part 4: Kanban Deep Dive

# The dispatcher

The Kanban dispatcher runs inside the Hermes gateway and polls every N seconds (default 60):

1. Reclaims stale claims (agent died mid-task)
2. Promotes ready tasks (all parent tasks done → promote)
3. Spawns assigned profiles as workers
4. Workers get `kanban_*` tools to interact with the board

# Workspace types

|Type|Use Case|Persistence|
|:-|:-|:-|
|`scratch`|One-off tasks|Deleted on completion|
|`dir:<path>`|Ongoing work in a real directory|Preserved|
|`worktree`|Git worktree for coding|Preserved|

# Multi-board

For separate projects, create separate boards:

    hermes kanban boards create project-alpha --name "Project Alpha"
    hermes kanban --board project-alpha create "Add payment integration" --assignee coder

# Kanban vs delegate_task — when to use which

    Use delegate_task when:              Use Kanban when:
    - Parent needs answer to continue    - Work crosses agent boundaries
    - No humans involved                  - Might need human input
    - Result goes into parent's context   - Needs to survive restarts
    - Short reasoning task (<50 turns)    - Multiple agents over task lifetime
    - Parallel research or code review    - Scheduled recurring work

# Part 5: Delegation Patterns (from Official Docs)

# Parallel Research

    delegate_task(tasks=[
        {"goal": "Research topic A", "toolsets": ["web"]},
        {"goal": "Research topic B", "toolsets": ["web"]},
        {"goal": "Research topic C", "toolsets": ["web"]}
    ])

# Code Review

    delegate_task(
        goal="Review src/auth/ for security issues",
        context="Project at /home/user/webapp. Python 3.11, Flask. Files: src/auth/login.py, src/auth/jwt.py. Test: pytest tests/auth/ -v",
        toolsets=["terminal", "file"]
    )

# Multi-File Refactoring

Split files across parallel subagents. Each gets separate terminal session. Don't let two subagents touch the same file.

# Gather Then Analyze

Use `execute_code` for mechanical data gathering (10+ tool calls cheaply), then `delegate_task` for the single expensive reasoning step with clean context.

# Critical: The Context Problem

Subagents know NOTHING about your conversation. Always pass explicit file paths, error messages, and constraints.

# Part 6: Common Pitfalls

# Pitfall 1: Profiles ≠ Sandboxes

"I thought creating a 'work' profile would isolate my work files from my personal agent." — Profiles isolate Hermes state, not filesystem access. Use Docker, VMs, or `terminal.home_mode: profile` for filesystem isolation.

# Pitfall 2: Agent-to-Agent Loops

Multiple agents talking to each other via Telegram/Discord can create infinite loops. Always set circuit breakers: `tool_loop_guardrails.hard_stop_enabled: true` with reasonable limits.

# Pitfall 3: Mixed Bot Tokens

If two profiles accidentally share a Telegram bot token, the second gateway gets blocked. Check `~/.hermes/profiles/<name>/.env` for each profile.

# Pitfall 4: Delegate_task for Durable Work

"Created a research task via delegate\_task, my laptop went to sleep, lost everything." — delegate\_task is synchronous. For durable work, use Kanban or cronjob.

# Pitfall 5: `SOUL.md` Changes in Active Sessions

Changes to SOUL.md take effect on NEW sessions only. If you change SOUL.md mid-conversation, the agent won't see it until /new.

# Pitfall 6: Confusing HERMES_HOME with Working Directory

Setting terminal.cwd does NOT set the profile boundary. The profile boundary is HERMES\_HOME. Working directory is separate.

# Part 7: FAQ

**Q: How do I separate work and personal agents?**
A: Create separate profiles: `hermes profile create work --clone`. Give each its own Telegram bot token. Or use Telegram topics with a single bot routed to different profiles.

**Q: Can agents talk to each other directly?**
A: Not natively like a group chat. The closest patterns are: (1) Kanban comments as inter-agent protocol, (2) Telegram group with multiple bot tokens, (3) A2A Context Bus plugin. Direct agent-to-agent conversation is not a built-in Hermes feature.

**Q: Should I use profiles or delegate\_task?**
A: Profiles = persistent agents with memory and identity. Delegate\_task = disposable subagents for one-off reasoning. They're complementary — profiles are the team, delegate\_task is consulting an expert for a quick opinion.

**Q: How many profiles can I run?**
A: No hard limit. Each profile is just a directory. Resource limits are the real constraint — each gateway consumes memory. Docker users: one container supervises all profiles via s6-overlay.

**Q: Can I delegate a task to a different profile?**
A: delegate\_task spawns anonymous subagents using the current model — NOT named profiles. This is a known limitation. At least 4 PRs have been opened to add a `profile` parameter to delegate\_task, but none have been merged as of June 2026 (earliest PR is 2+ months old — #6771). Workarounds: (1) Use Kanban (create task → assignee picks it up), (2) Use auxiliary model config for specific tasks (e.g., set a vision model as `auxiliary.vision`), (3) The constitution pattern (orchestrator reads constitution, manually routes), (4) Instruct the agent to call `hermes -p <profile> chat -q "..."` from the CLI (messy but works).

**Q: How do I share memory across profiles?**
A: Honcho memory provider shares a user workspace across profiles by default. Each profile builds independent observations. For custom sharing, use a shared Obsidian vault, shared files, or Kanban comments as context transfer.

**Q: What are profiles NOT good for?**
A: Profiles are not containers. If you need strict filesystem isolation between agents, use separate Docker containers, VMs, or machines.

**Q: Can one Telegram bot serve multiple profiles?**
A: Yes — via Telegram topics. Each topic routes to a different profile. Enable topics in BotFather, configure routing in gateway config. Simpler than managing multiple bot tokens.

**Q: My agents keep getting stuck in loops when coordinating. What do I do?**
A: Enable hard stops: `tool_loop_guardrails.hard_stop_enabled: true` with `hard_stop_after.exact_failure: 5`. Also set Kanban failure limits (`kanban.failure_limit`) to prevent thrashing on broken tasks.

# Part 8: Knowledge Table — Every Tool & Pattern

|Name|Type|Best For|Watch For|
|:-|:-|:-|:-|
|**Profiles**|Multi-agent primitive|Persistent agents with identity|Not filesystem sandboxes|
|**Kanban**|Task queue|Durable multi-agent coordination|Dispatcher runs in gateway|
|**delegate\_task**|Subagent spawn|Parallel research, code review|Not durable; lost on restart|
|**Telegram Topics**|Message routing|One bot → multiple profiles|Needs BotFather topic setup|
|**Constitution Pattern**|Community invented|Orchestrator routing rules|Manual setup, no built-in tool|
|**A2A Context Bus**|Community plugin|Agent message passing|Custom build required|
|**Honcho Memory**|Memory provider|Shared memory across profiles|Setup complexity|
|**Kanban Comments**|Inter-agent protocol|Agent-to-agent handoff|Agents must be instructed to read|
|**Kanban Workspaces**|Task isolation|Per-task directories|Scratch dirs deleted on completion|
|**Multiple Bot Tokens**|Gateway isolation|Strict agent separation|More tokens to manage|

# Part 9: The Bottom Line

Hermes profiles + Kanban form a genuine multi-agent operating system — not a chatbot with a "team mode" gimmick. Each profile is a persistent agent with its own memory, skills, and identity. Kanban gives them a shared work queue that survives crashes and restarts.

The community is actively graduating from single-agent setups to agent teams: orchestrator + specialists pattern, shared boards with self-management, and increasingly sophisticated coordination via comments and context buses.

Start simple: create one extra profile, give it a different SOUL.md, connect it to a Kanban board. Watch how the orchestrator naturally starts routing tasks.

*Synthesized from 8+* r/hermesagent *threads, official Hermes profiles documentation, delegation patterns guide, Kanban specification, and community-reported setups. Corrections and additions welcome.*

---

## Comment-Sourced Updates

- **Profiles are not sandboxes:** they isolate configuration, sessions, memory, and credentials; ordinary filesystem permissions still apply.

- **Delegation is not durable scheduling:** a background child tied to the parent session can be lost when that session closes. Use Kanban or another tracked durable mechanism for work that must survive interruption.

- **Table repair:** the Reddit rendering obscured a comparison-table column. This source copy should be reviewed as profiles/runtime overrides/delegation/Kanban—not as interchangeable mechanisms.

- **Avoid profile proliferation:** runtime model overrides and one-shot profile commands often solve temporary routing needs without creating another permanent identity.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
