# Kanban Setups Megathread — Hermes Agent (June 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

**Original post last updated:** June 26, 2026
**Sources used by the original post:** Official Hermes Kanban documentation, the Kanban tutorial, the kanban-orchestrator skill, 15+ r/hermesagent threads, Nous Research/community posts, release notes through v0.17, external guides, and the earlier multi-agent megathread.

---

## TL;DR — Quick Reference

| Decision | Pick | Why |
|----------|------|-----|
| First-time Kanban setup | **Hermes' built-in Kanban** (zero-config) | Ships with every install — `hermes kanban init` |
| Starting profiles | **Orchestrator + 1 specialist** | You don't need 5 profiles on day one |
| Kanban vs delegate_task | **Kanban for durable work** | Survives crashes, has human-in-loop, audit trail |
| Best Kanban model | **Same as your daily driver** (auxiliary decomposer can be smaller) | Kanban workers use the assigned profile's model |
| Board isolation | **One board per project** | Only needed when workstreams collide |
| SOUL.md strategy | **One per profile** | Define role, not behavior — let the dispatcher orchestrate |
| Config trap #1 | **`failure_limit` too low** | Default 2 — bump to 5 if workers crash on first attempt |
| Config trap #2 | **No `HERMES_KANBAN_TASK` env check** | Workers can't see their task without it |
| Dashboard | `hermes dashboard` → Kanban tab | Live WebSocket updates, drag-drop, drawer |

---

## Part 1: What Is Kanban (and Why It's Different)

Kanban is a **durable SQLite-backed task board** shared across all your Hermes profiles. Not a RPC call like `delegate_task` — it's a persistent message queue where every handoff is a row anyone can see and edit.

### The key distinction

| Aspect | `delegate_task` | Kanban |
|---|---|---|
| Shape | RPC call (fork → join) | Durable message queue + state machine |
| Parent | Blocks until child returns | Fire-and-forget after `create` |
| Child identity | Anonymous subagent | Named profile with persistent memory |
| Resumability | None — failed = failed | Block → unblock → re-run; crash → reclaim |
| Human in the loop | Not supported | Comment / unblock at any point |
| Agents per task | One call = one subagent | N agents over task's life (retry, review, follow-up) |
| Audit trail | Lost on context compression | Durable rows in SQLite forever |
| Coordination | Hierarchical (caller → callee) | Peer — any profile reads/writes any task |

**Use `delegate_task` when:** parent agent needs a short reasoning answer, no humans involved, result goes back into parent's context.
**Use Kanban when:** work crosses agent boundaries, needs to survive restarts, might need human input, or you want a discoverable audit trail.

### How it works at a glance

```
You create a task → dispatcher polls → spawns assigned profile as worker
    → worker calls kanban_show(), does work, calls kanban_complete()
    → child tasks auto-promote from todo → ready when parents finish
    → next worker picks up ready task → repeat
```

Three surfaces, one `kanban.db`:
- **Workers** drive the board through `kanban_*` tools (`kanban_show`, `kanban_complete`, `kanban_block`, `kanban_heartbeat`, `kanban_comment`, `kanban_create`, `kanban_link`)
- **You** drive it through `hermes kanban …` CLI or `/kanban …` slash command
- **Dashboard** drives it through drag-drop and inline forms

---

## Part 2: Profiles — Your Kanban Team

Kanban is built on profiles. Every task gets an `--assignee <profile>`, and the dispatcher spawns that profile as a worker.

### Recommended minimum setup

```
Profile: default (orchestrator)
  Role: Routes work, creates tasks, doesn't do the work itself
  Toolsets: kanban, gateway, memory (no terminal/file)
  Model: Your strongest reasoning model
  SOUL.md: Orchestrator routing instructions

Profile: worker (specialist)
  Role: Implements, researches, writes, reviews
  Toolsets: terminal, file, web (whatever the work needs)
  Model: Works well with tools
  SOUL.md: "You are a [role]. When spawned as a kanban worker..."
```

### The orchestrator role

The orchestrator is a profile that **decomposes goals into tasks and routes them** but does not do the work itself. The official docs call this out explicitly:

> "A well-behaved orchestrator does not do the work itself. It decomposes the user's goal into tasks, links them, assigns each to one of the profiles you've set up, and steps back."

The official kanban-orchestrator guidance (auto-injected) includes:
- Anti-temptation rules ("Do not execute the work yourself")
- Step 0: discover available profiles before planning
- Decomposition playbook: sketch task graph → create cards with dependencies → link → complete

**Which profile owns decomposition?** Controlled by `kanban.orchestrator_profile` in config. If unset, falls back to the active default profile.

### Profile descriptions matter

The decomposer reads profile descriptions to route work. Set them:

```bash
hermes profile describe researcher --text "Web research, document analysis, API exploration"
hermes profile describe coder --text "Python, JavaScript, system scripts, refactoring"
hermes profile describe researcher --auto   # LLM generates from installed skills
```

Without descriptions, the decomposer can still route by profile name but less precisely. If it picks an unknown profile name, the child routes to `kanban.default_assignee` (or the active default).

### Profile Builder (v0.16+)

Since v0.16 (June 2026), you can build complete profiles visually in the dashboard:

1. Open `hermes dashboard` → Profiles section
2. Click "New Profile" — name, description, model, SOUL.md, toolsets, enabled skills
3. The dashboard generates the profile directory and config.yaml automatically
4. Profile descriptions are also editable in the dashboard (the decomposer reads them)

No more `hermes profile create --clone` and manual config edits for basic setups. The dashboard also shows all profiles with their assigned Kanban task counts per profile.

### Common profile rosters from the community

**The 2-profile starter:**
- `default` (orchestrator — routes, never implements)
- `worker` (does everything)

**The 3-profile standard:**
- `orchestrator` (routes, decomposes)
- `researcher` (web research, data gathering)
- `implementer` (coding, writing, execution)

**The 5-profile fleet:**
- `orchestrator`
- `researcher`
- `coder`
- `writer`
- `reviewer`

⚠️ **Don't build the org chart on day one.** Start with 2 profiles. Add specialists only when you actually hit context pollution or routing confusion. Most users never need more than 4 profiles.

---

## Part 3: SOUL.md Configuration for Kanban

Each profile in your Kanban team needs a SOUL.md that defines its role in the board workflow. Here's the difference between an orchestrator SOUL.md and a worker SOUL.md.

### Orchestrator SOUL.md (the router)

```yaml
# ~/.hermes/profiles/default/SOUL.md
personality: Orchestrator / routing agent
tone: Professional, clear, efficient
---
You are the Kanban orchestrator. Your job is to decompose the user's goals into tasks, assign them to specialist profiles via the kanban board, and report back. You do NOT do the work yourself.

## Rules
1. When the user asks for anything, first decide: is this a quick answer I can give directly, or does it need specialist work?
2. If it needs specialist work, break it into parallel tasks where possible. Use kanban_create with assignee, title, body, and optional parents.
3. Assign to profiles that actually exist. Run `hermes profiles list` if unsure.
4. After creating tasks, tell the user what you queued and who's assigned.
5. Do not implement. Do not research. Do not write code. Route only.

## Available specialist profiles
- `researcher` — web research, API docs, data gathering
- `coder` — Python, JS, system scripts, GitHub
- `writer` — drafting, editing, prose
- `reviewer` — code review, quality checks
```

### Worker SOUL.md (the implementer)

```yaml
# ~/.hermes/profiles/worker/SOUL.md (for a generalist worker)
personality: Task executor
tone: Practical, direct
---
You are a kanban worker. When spawned for a task, follow this lifecycle:

1. Call kanban_show() to read your task's title, body, and parent handoffs.
2. Change to $HERMES_KANBAN_WORKSPACE directory.
3. Do the work: research, code, write, whatever the task requires.
4. Call kanban_heartbeat(note="...") during long operations (at least once/hour).
5. Complete with kanban_complete(summary="...", metadata={...}).

## Handoff quality
- summary: human-readable closeout
- metadata: machine-readable handoff (changed_files, verification, residual_risk)
- If stuck, call kanban_block(reason="...") instead of completing
```

### Community SOUL.md patterns

- **"Split personality" pattern** (u/Starrwulfe): One profile, multiple personalities via skill files, routed by constitution.md. Collapses 5 agents into 1.
- **"Strict role enforcement"** (u/rk1213): Each profile gets explicit role boundaries — "You are ONLY a researcher/coder/reviewer. Never step outside your role."
- **"Temperature as parameter"** (multiple users): Lower temp (0.3-0.5) for workers (predictable tool use), higher (0.7-1.0) for orchestrators (creative decomposition).
- **Model-specific SOUL.md notes:** Users running local models often add disclaimers like "You run on Qwen3-27B at Q4_K_M. You have 32K context. Work within these constraints."

---

## Part 4: Config.yaml — The Kanban Section

All Kanban config lives under `kanban:` in `~/.hermes/config.yaml`:

```yaml
kanban:
  # Dispatcher
  dispatch_in_gateway: true          # runs inside gateway process (default)
  dispatch_interval_seconds: 60      # poll interval (default)
  dispatch_stale_timeout_seconds: 14400  # 4h before reclaiming stale workers
  failure_limit: 2                   # circuit breaker trips after N failures

  # Concurrency
  max_in_progress: ~                  # unset = unlimited (default)
  max_in_progress_per_profile: ~      # per-profile cap (default = unlimited)

  # Triage / decomposition
  auto_decompose: true                # dispatcher auto-runs decomposer (default)
  auto_decompose_per_tick: 3          # max decompositions per tick
  auto_promote_children: true        # auto-promote decomposed children to ready
  orchestrator_profile: ""            # profile for root orchestration task
  default_assignee: ""                # fallback when LLM picks unknown profile
  default_workdir: ~                  # board-level default workspace dir

  # Subscriptions
  auto_subscribe_on_create: true      # auto-subscribe origin to task events
```

### Tuning advice from the community

- **`failure_limit`:** Default 2 is aggressive. If your workers use local models that occasionally crash on first attempt (OOM, segfault), bump to 5. The circuit breaker is a safety net, not a quality gate.
- **`dispatch_interval_seconds`:** 60s is fine for most. Drop to 10-15s if you want snappy response for short tasks. Keep at 60s+ if workers take minutes (local LLM, slow API).
- **`max_in_progress`:** Critical for local-model setups. If you're running a 7B model on a laptop, cap to 1-2 so you don't OOM. Unset = unlimited.
- **`auto_decompose`:** Great for the "drop a one-liner, walk away" flow. Turn off if you want full manual control — triage tasks sit in triage until you hit the ⚗ Decompose button.
- **`auto_promote_children: false`:** Use if you want to review decomposed tasks before they get picked up. Children stay in `todo` until you promote them.

### Auxiliary LLM slots

```yaml
auxiliary:
  kanban_decomposer:
    model: anthropic/claude-sonnet-4    # or local model
    provider: anthropic
  profile_describer:
    model: custom:lmstudio/gemma-4-12b-it  # lightweight model for this
```

- `kanban_decomposer`: Model that produces the task graph. Can be a different/smaller model than your main chat model. Set it if your main model is slow or expensive for decomposition calls.
- `profile_describer`: Model that auto-generates profile descriptions (`hermes profile describe --auto`). A small local model is fine here.

---

## Part 5: Models for Kanban Workers

The model choice per profile dramatically affects Kanban performance.

### Guidelines

| Worker type | Recommended model | Why |
|---|---|---|
| Orchestrator | Strong reasoning (DeepSeek V4, Claude Sonnet 4, Qwen3-27B) | Decomposition quality depends on task understanding |
| Researcher | Strong web-reading, medium size | Cost efficiency; doesn't need top reasoning |
| Coder | Local Qwen3-27B or Qwen3.5-9B | Tool-calling reliability matters |
| Writer/translator | Small local (Gemma-4-12B, Qwen3.5-9B) | These are serialization tasks |
| Reviewer | Same as coder but different SOUL.md | Needs same model quality for correctness |

### Known issues

- **Local model workers on slow GPUs:** Workers time out before finishing. Fix: set `max_in_progress: 1` so only one worker runs at a time, and raise `dispatch_stale_timeout_seconds`.
- **Cloud model costs burn fast:** Each dispatcher spawn creates a new session. A task that takes 20 turns of Claude costs $0.60-1.20 per run. Use local models for high-volume workers.
- **Model switching per profile:** Each profile has its own model config. You can route a task to a profile running DeepSeek and another to a profile running a local Qwen3 — within the same Kanban board.

### Context window considerations

- Workers inherit the profile's config, including context window settings.
- Kanban adds overhead via `kanban_show()` output (task title, body, parent handoffs, prior attempts, comments). For tasks with long comment threads or many prior attempts, the context payload can be 2-8K tokens.
- **Goal-mode cards** (`--goal`) add per-turn judge overhead: the auxiliary model evaluates completion after every turn. Budget accordingly.

---

## Part 6: Tools and Skills for Kanban Workers

### Built-in kanban toolset

Every dispatcher-spawned worker automatically gets these tools. No manual configuration required — `HERMES_KANBAN_TASK` env var flips them on:

| Tool | Purpose |
|---|---|
| `kanban_show` | Read current task (title, body, parent handoffs, comments, prior attempts) |
| `kanban_list` | List tasks by assignee/status/tenant (orchestrators) |
| `kanban_complete` | Finish task with summary + metadata |
| `kanban_block` | Stop work with a reason + block kind |
| `kanban_heartbeat` | Liveness signal during long ops |
| `kanban_comment` | Append durable note to task thread |
| `kanban_create` | (Orchestrators) Fan out into child tasks |
| `kanban_link` | (Orchestrators) Add dependency edge |
| `kanban_unblock` | (Orchestrators) Move blocked task back to ready |

### Pin extra skills per task

You can attach skills to individual tasks without editing the profile:

```bash
hermes kanban create "Translate README to JP" \
    --assignee linguist --skill translation

hermes kanban create "Audit auth flow" \
    --assignee reviewer --skill security-pr-audit --skill github-code-review
```

Workers get these skills loaded on top of their profile's defaults. Great for one-off specialist tasks without profile bloat.

### What not to give a kanban worker

- **Don't give orchestrators terminal/file tools** — they'll start implementing instead of routing. Restrict to `kanban`, `gateway`, `memory`.
- **Don't overload workers with irrelevant skills** — each skill increases startup time and context overhead. Keep worker profiles lean (15-20 skills maximum per the community's gradual-approach advice).

---

## Part 7: Configuration Parameters — Full Reference

### CLI create flags

```bash
hermes kanban create "<title>" \
    --body "..."                    # Task description
    --assignee <profile>            # Who does this work
    --parent <id>                   # Dependency (repeat for multiple)
    --tenant <name>                 # Namespace for multi-business setups
    --workspace scratch|worktree|dir:<path>  # Working directory type
    --branch <name>                 # Git branch for worktree workspaces
    --priority N                    # 1-5 (higher = more important)
    --triage                        # Park in triage column first
    --idempotency-key KEY           # Dedup for cron/automation
    --max-runtime 30m|2h|1d         # Hard timeout per run
    --max-retries N                 # Circuit breaker threshold (overrides failure_limit)
    --goal                          # Goal-mode (judge evaluates completion)
    --goal-max-turns N              # Max turns in goal mode (default 20)
    --skill <name>                  # Pin skill to this task (repeat for multiple)
```

### Key config knobs

| Config key | Default | What it does |
|---|---|---|
| `kanban.dispatch_interval_seconds` | 60 | How often dispatcher polls for ready tasks |
| `kanban.dispatch_stale_timeout_seconds` | 14400 (4h) | Reclaim workers with no recent heartbeat |
| `kanban.failure_limit` | 2 | Circuit breaker trips after N consecutive failures |
| `kanban.max_in_progress` | unset (unlimited) | Board-wide concurrency cap |
| `kanban.max_in_progress_per_profile` | unset (unlimited) | Per-profile concurrency cap |
| `kanban.auto_decompose` | true | Auto-run decomposer on triage tasks |
| `kanban.auto_decompose_per_tick` | 3 | Max decompositions per dispatcher tick |
| `kanban.auto_promote_children` | true | Auto-promote decomposed children to ready |
| `kanban.orchestrator_profile` | "" | Profile for root orchestration task (empty = active default) |
| `kanban.default_assignee` | "" | Fallback assignee for unknown profile names |
| `kanban.default_workdir` | unset | Board-level default workspace directory |
| `auxiliary.kanban_decomposer` | same as main | Model for task decomposition |
| `auxiliary.triage_specifier` | same as main | Model for triage → todo spec writing |
| `auxiliary.profile_describer` | same as main | Model for auto-generated profile descriptions |

---

## Part 8: Community Setups and Patterns

### The 6-Profile Production Pipeline (u/rk1213)

One of the most detailed community setups posted — a self-governing pipeline for overnight coding work:

> **"there's two main purposes for the kanban for me: 1) guardrails and checks. I always have a profile that reviews/audits work using a different model family. 2) automation. I give the team a job and I can still use my agent as normal. I can give the team a full coding job before I sleep and then wake up with the job completed."**

The 6 profiles:
1. **orchestrator** — handles routing tasks to the right agent
2. **research/analyst** — searches for relevant information (tools, APIs, example workflows)
3. **planner** — writes implementation plans with a template, incorporates research
4. **plan reviewer** — first guardrail; reviews the plan, checks for hallucinations
5. **coder** — implements based on the approved plan
6. **code reviewer** — second guardrail; checks for errors, runs tests

> "The whole kanban pipeline circulates until task is completed. I have a watcher that notifies me if there is anything that specifically needs my approval or input. If none, the whole end to end process is pretty much self governing."

Pipeline output example from an overnight run:

```
✅ Orchestrator decomposed Phase 2
✅ Research analyst gathered codebase context
✅ Planner wrote the implementation plan
✅ Plan reviewer reviewed it
✅ Orchestrator routed the review
🔄 Planner is now fixing the plan (Revision 1) — reviewer had feedback
⏳ Plan re-review queued
⏳ Orchestrator acceptance queued
```

> "So far, I won't say it can complete everything without errors or gets everything 100% the way I want it every time, but it's about 70-80% there."

**Key insight:** "By default, these agents won't have access to the skills/toolsets that you have built for your main agent. You will need to give these agents relevant skills and toolsets that they require in order to function properly."

### The Two-Specialist Starter (u/itsdodobitch, mod)

The top-voted comment in the recent Kanban discussion thread (+16) explains the fundamental difference:

> "Profiles are not just about personality. A profile defines what your agent knows, what it remembers, which tools it has access to, which skills it can use (and automatically edit), and its memory. Each profile has its own set of files because a profile is literally a nested folder that mirrors the default structure."

Recommended starter split:
- One profile with only terminal and file operations
- Another with web and browser access for research

> "With Kanban, your agent can create a task, including one much larger and more complex than what you would usually assign to a delegate-task sub-agent, and assign it to a specific profile. A dispatcher then checks every 60 seconds for pending Kanban tasks and moves the board accordingly."

### The Visual-LLM Split (u/H4llifax)

> "I'm using a text-only model as my main model, and I have a VLM as second profile for vision related tasks. It's a bit awkward that delegate_task can't delegate to other profiles."

This is a concrete use case where Kanban uniquely solves a problem: when your main model is text-only, Kanban lets you route vision tasks to a profile with a vision model — something `delegate_task` can't do.

### The 46-Profile Fleet (u/Bulky_Magician)

> "I have 3 separate workflows with routing profiles for each. Overall I think I have 46 profiles right now. It's worked out very well but I do feel like I need to become stricter on routing profiles remembering all use cases for downstream profiles."

⚠️ **Counterpoint from the same user:** "Despite putting time and effort into setting it up, I've just not been able to get into using kanban or even really cron jobs. I don't have a frontier model, just owl alpha, and so far I feel like I need to babysit it."

This is a recurring theme — Kanban with weaker local models requires more tuning and tolerance for imperfect automation.

### From X/Twitter and Hermes Release Notes

**Nous Research Kanban announcement** (May 3, 2026, v0.12.0 launch) — the official announcement post on X hit:
- **1.5M views, 5.9K likes, 492 reposts, 270 replies**
- Describes Kanban as: "Agents claim tasks from a board, work in parallel, and hand off when blocked. You watch progress and unblock from one easy view instead of juggling terminals."
- The 270 replies contain user setups, questions, and tuning advice beyond what's captured in this megathread

**v0.15 "Velocity" Release (May 28-29, 2026):**
- "Kanban and multi-agent work got more serious" — continued platform push toward durable multi-agent task graphs with workers, verifiers, and swarms
- v0.15.1 fixed **Kanban worker SIGTERM handling** — previously, sending SIGTERM to a worker could orphan the task in the database; patch adds a graceful cancel with failure reason code
- "stronger Kanban orchestration" listed as upgrade reason
- Core `run_agent.py` refactored from 16K to 4K lines for safer future changes

**v0.16 "Surface" Release (June 8, 2026):**
- **Profile Builder** in the dashboard — build complete profiles visually (SOUL.md, tools, skills, model) without touching config files — @shannholmberg on X
- Dashboard expanded for MCP catalog, channels, credentials, webhooks, memory
- Remote backend support (desktop connects to VPS gateway)
- Model picker improvements across all surfaces

**v0.17 "The Reach Release" (June 19, 2026):**
- **Background subagents** — directly impacts Kanban workers; subagents can now run as detached background processes
- iMessage and WhatsApp integrations via Photon Spectrum
- ~1,475 commits, ~800 merged PRs since v0.16 — significant churn

**From the broader X community (June 2026):**
- **@Saboo_Shubham_** (May 22): "Ultimate Guide to running Free Local Coding Agents with Hermes" — SmallCode + Kanban for free local coding agent pipelines
- **@ScottyBeamIO** (Jun 16): Full architecture guide detailing kanban_decomposer setup and multi-agent patterns
- **The Goldie Self-Driving Board** (@agentos.guide): Community pattern for auto-managing Kanban boards with an orchestrator that rebalances tasks, detects stuck workers, and routes around failures
- **Hermes Agent Community on X** (7.8K members): Active discussions on profile building, SOUL.md design, and Kanban pipeline patterns

> **Note:** Several of these sources were extracted via search snippets and browser — some blog post content may have additional detail not captured here. Community members are encouraged to link deeper resources in the comments.

### The 2-Profile Starter (recommended for everyone)

The most common advice across threads — start with one orchestrator and one generalist worker:

```yaml
# config.yaml
kanban:
  orchestrator_profile: default
  auto_decompose: true
```

```bash
hermes profile create worker --clone
# Edit worker's config: restrict tools, set model
```

### The 4-tier scaling model (u/nemanja87mn)

| Tier | Setup | When |
|------|-------|------|
| **Tier 1** | One agent, delegate_task for subagents | Protecting context without adding complexity |
| **Tier 2** | Orchestrator + specialist profiles, one chat surface | Multiple roles, isolated memory |
| **Tier 3** | Domain orchestrators with separate bots | Different businesses/domains |
| **Tier 4** | Separate instances (same machine or VPS) | Client work, production, strict isolation |

Most people never need Tier 3 or 4.

### Goal-mode: "keep going until done"

```bash
hermes kanban create "Translate the whole docs site to French" \
    --body "Acceptance: every page translated, no English left, links intact." \
    --assignee translator \
    --goal --goal-max-turns 15
```

Workers keep running in the same session, with an auxiliary judge checking completion after every turn. Budget exhausted without done → task blocks for human review (never silent exit).

### Kanban Swarm (v1)

```bash
hermes kanban swarm "Design a multi-region failover plan" \
    --workers researcher,architect,sre \
    --verifier reviewer --synthesizer writer
```

Creates: root/blackboard card → N parallel workers → verifier gated on all workers → synthesizer gated on verifier. All normal dispatcher lifecycle.

### Multi-board for separate projects

```bash
hermes kanban boards create project-alpha \
    --name "Project Alpha" --description "Client work" --icon 🏗️

hermes kanban --board project-alpha list
```

Per-board isolation: separate SQLite DB, workspaces, logs. Workers see only their board's tasks.

### The Constitution pattern (community-invented)

A `constitution.md` document that defines each profile's specialty. The orchestrator reads it before routing:

```markdown
## Agent: researcher
- Specialty: web research, docs reading, API exploration
- Tools: web, file
- Route: any research or discovery task

## Agent: coder
- Specialty: Python, JavaScript, system scripts
- Tools: terminal, file, github
- Route: any coding or implementation task
```

Orchestrator's SOUL.md: "Read constitution.md before routing. Assign to the profile whose specialty matches."

### Multi-tenant setups

```bash
hermes kanban create "monthly report" \
    --assignee researcher \
    --tenant business-a \
    --workspace dir:~/tenants/business-a/data/
```

Workers receive `$HERMES_TENANT` and namespace memory writes by prefix. Same board, same dispatcher, scoped data.

---

## Part 9: Pitfalls and Fixes

### "Kanban tasks never get picked up"

- **Dispatcher not running:** The gateway must be running — `hermes gateway start`. The dispatcher runs inside the gateway by default. Without it, ready tasks stay in ready forever.
- **Profile doesn't exist:** `hermes kanban create "task" --assignee nonexistent-profile` — the dispatcher silently fails. Check with `hermes profiles list`.
- **Config error in profile:** Spawn fails, circuit breaker trips. Check `hermes kanban runs <id> --json` for the error.

### "Kanban DB got corrupted"

Multiple users reported SQLite corruption in v0.14.0. The root cause was identified as **index corruption** — `idx_tasks_status` and `idx_tasks_assignee_status` out of sync from concurrent subagent writes, not data loss.

**Fix (preserves all tasks and events):**
```sql
-- In ~/.hermes/kanban.db (or boards/<slug>/kanban.db)
REINDEX idx_tasks_status;
REINDEX idx_tasks_assignee_status;
```

**Workarounds from the community:**
- Create a dedicated subagent to serialize Kanban DB writes (single point of access) — u/drmembrane
- Switch orchestrator from auto to manual mode — u/arugrat11
- Migrate Kanban to Postgres (long-term; Honcho already uses Postgres) — u/drmembrane
- If all else fails: `kanban gc --event-retention-days 7` then reinitialize

> Multiple users reported: "Kanban currently non-usable because constant db corruptions" — u/qettyz, u/GuCaWa, u/Supernovali

**Prevent:** Back up `~/.hermes/kanban.db` and boards periodically. Use `kanban gc` to trim old events.

### "Kanban deleted my projects"

From a community report: setting a project folder as the working directory caused Kanban to create/delete scratch workspaces there. **Never use scratch workspaces in a production directory.** Use `dir:<path>` to pin an existing directory — scratch dirs are deleted on task completion.

> "Kanban finished working and deleted its own work too! Moron kanban." — u/Popular-Penalty6719

**Rule of thumb:** Always set working and outcome directories OUTSIDE your own folder structure. Either pin explicit `dir:<path>` for persistence or use an immutable flag on critical directories (`chflags uchg /your/protected/directory` on macOS).

### "Infinite dispatch loop burning API tokens"

If approval_mode is set to `manual` and a worker hits an SSH/SCP command that goes to `pending_approval`, the worker fails and crashes. The dispatcher spawns a NEW worker instantly, creating an infinite loop. One user reported 14 dispatches for the same task in ~90 minutes, each consuming API tokens.

**Fix:** Configure workers to enter `kanban_block(pending)` status instead of failing on blocked commands. Or set `approval_mode: auto` if you trust the commands being issued.

### "Worker results don't return to orchestrator"

When a worker completes a task, the result doesn't always come back to the orchestrator's gateway channel. This is a known gap — the worker's `kanban_complete` writes to the DB, but there's no built-in mechanism to surface the result back to the orchestrator's chat.

**Workaround:** Subscribe to the task's terminal events:
```bash
hermes kanban notify-subscribe <task-id> --platform telegram --chat-id <id>
```
Or set up a watcher: `hermes kanban watch --kinds completed`

### "Workers keep timing out / crashing"

- **Local model too slow:** Cap `max_in_progress` to prevent overload.
- **Model doesn't follow tool protocol:** Worker exits without calling `kanban_complete`. The dispatcher emits `protocol_violation` and blocks the task. **Fix:** Use a model that reliably calls tools, or include explicit instructions in the worker's SOUL.md.
- **OOM on large tasks:** Workers spawn with the profile's context window. If the task description + parent handoffs are huge, consider smaller context or chunking.

### "Triage → todo promotion issues"

- If `auto_decompose: true` but no decomposer is configured, triage tasks get stuck. Configure `auxiliary.kanban_decomposer` or switch to manual mode.
- If a task in `todo` without an assignee auto-promotes to `ready`, that's by design — the dispatcher picks up unassigned tasks using `kanban.default_assignee` or the active default profile.

### "Kanban feels complex"

From community threads comparing Hermes Kanban to simpler systems (Paperclip, NanoClaw): the complexity trade-off is **durability + audit trail**. Hermes Kanban is an industrial-grade task board, not a lightweight todo list. If you don't need retry history, human-in-loop, or multi-agent coordination, use `delegate_task` instead.

### "Agent keeps loop-de-looping with Kanban"

**Hard stop guard:** Set `tool_loop_guardrails.hard_stop_enabled: true` with `hard_stop_after.exact_failure: 5`. Also set per-task `--max-retries` to prevent infinite retry loops.

### "I want Kanban on multiple machines"

Kanban is single-host by design. `~/.hermes/kanban.db` is a local SQLite file. For multi-host setups, run an independent board per host and bridge them with `delegate_task` or a message queue.

---

## Part 10: FAQ

**Q: How do I start using Kanban today?**
A: `hermes kanban init` → `hermes gateway start` → `hermes kanban create "my first task" --assignee <profile>`. The dashboard at `hermes dashboard` → Kanban tab shows the board.

**Q: Do I need profiles to use Kanban?**
A: Yes. Kanban tasks must be assigned to a profile. You can use your default profile for everything, but that defeats the purpose — the strength is routing work to specialised profiles.

**Q: Can a single agent work Kanban tasks?**
A: Yes. A single profile can claim tasks, work them, and complete them. The dispatcher serialises — one task at a time for that profile.

**Q: What's the difference between a kanban worker and a normal chat session?**
A: Workers spawn with `HERMES_KANBAN_TASK` env var, which enables the `kanban_*` toolset and injects the worker lifecycle guidance. A normal `hermes chat` session doesn't have these.

**Q: Can I use Kanban with only local models?**
A: Yes. Many users run fully local Kanban fleets. Key tuning: set `max_in_progress` to match your GPU memory, raise `dispatch_stale_timeout_seconds` for slow models, and use a smaller model for the decomposer.

**Q: How do retries work?**
A: When a worker fails (doesn't call `kanban_complete`), the dispatcher reclaims the task and tries again. After `failure_limit` consecutive failures, the circuit breaker trips and the task auto-blocks. You unblock it from the dashboard or CLI, and the retry counter resets.

**Q: How do I see what went wrong on a failed task?**
A: `hermes kanban runs <id>` shows every attempt with outcome, error, and summary. `hermes kanban log <id>` shows the worker's stdout/stderr for debugging.

**Q: Can I attach files to Kanban tasks?**
A: Yes — upload from the dashboard drawer (25 MB cap per file). Workers see attached files as absolute paths in their context.

**Q: What's the best model for the decomposer?**
A: The decomposer doesn't need to be your strongest model — it just produces a task graph. A 12-27B local model works fine. On slow decomposers, crank up `dispatch_interval_seconds` so it doesn't block the dispatcher.

---

## Part 11: Knowledge Table

| Feature | What It Is | Best For | Watch For |
|---------|-----------|----------|-----------|
| **Built-in Kanban** | SQLite board, zero-config | Multi-agent durable coordination | Dispatcher must be running |
| **Profiles** | Separate Hermes identities | Agent team with distinct memory/skills | Not filesystem sandboxes |
| **Orchestrator pattern** | Router profile, no implementation | Task decomposition and routing | Don't give it terminal tools |
| **Goal-mode** | Judge evaluates per-turn completion | Open-ended multi-step tasks | Auxiliary LLM cost per turn |
| **Decomposer** | Auto-splits triage tasks into graphs | "Drop a one-liner, walk away" | Needs configured auxiliary model |
| **Scratch workspace** | Temp dir, deleted on completion | One-off tasks | NEVER use for production data |
| **dir: workspace** | Pinned directory | Ongoing work | Must be absolute path |
| **worktree workspace** | Git worktree | Coding tasks | Creates real git branches |
| **Multi-board** | Isolated per-project boards | Separate workstreams | Confusion if you forget `--board` |
| **Skill pinning** | Per-task skills | One-off specialist context | Skill must exist on target profile |
| **Circuit breaker** | Auto-blocks after N failures | Prevents worker thrashing | Default 2 may be too aggressive |
| **Heartbeat** | Liveness signal | Long operations (>1 hour) | Reclaim after 1h without heartbeat |
| **Scheduled tasks** | `scheduled_at` timestamp | Nightly ops, timed runs | UTC timestamps |
| **Kanban Swarm** | Built-in swarm topology | Parallel workers + verifier | New feature, less battle-tested |
| **Telegram notifications** | Auto-subscribe on `/kanban create` | Mobile notifications | Gateway must be running |

---

## Part 12: Quickstart — From Zero to Running Kanban in 5 Minutes

```bash
# 1. Create a worker profile (if you don't have one)
hermes profile create worker --clone
# Edit ~/.hermes/profiles/worker/config.yaml — restrict toolsets, set model

# 2. Init kanban + start gateway
hermes kanban init
hermes gateway start

# 3. Create your first task
hermes kanban create "Research local LLM options for a 16GB Mac" \
    --body "Focus on: GGUF models that fit 16GB, tool-calling reliability, Qwen3-9B vs Gemma-4-12B. Sources: reddit, huggingface, official benchmarks." \
    --assignee worker

# 4. Watch it happen
hermes kanban watch

# 5. Check the board
hermes kanban list
hermes kanban stats
```

---

*Synthesized from official Hermes Kanban documentation, the Kanban tutorial (4 stories), the kanban-orchestrator skill, 15+ r/hermesagent threads, v0.15/v0.16/v0.17 release notes, X/Twitter posts from @NousResearch, @shannholmberg, @Saboo_Shubham_, @ScottyBeamIO, and the Hermes Agent Community on X (7.8K members). Community quotes and setups from u/rk1213, u/itsdodobitch, u/H4llifax, u/Bulky_Magician, u/Krogg, u/veganmaister, u/Zenatic, u/JobOdd7262, u/nemanja87mn, u/Starrwulfe, u/Kromi75, u/Sufficient-Rip-6219, u/drmembrane, u/Popular-Penalty6719, u/Acrobatic-Let8742, u/Nazmul-TechTips, u/Technical_Win_3568, and others. Corrections and additions welcome in the comments.*

---

## Comment-Sourced Updates

- **Keep the team small:** every additional profile/worker carries startup context and coordination overhead. Add profiles for durable responsibility, credentials, memory, or routing boundaries—not as decoration.

- **Worker handoffs:** commenters recommend a worker-specific `AGENTS.md` or equivalent handoff document, explicit completion instructions, and clear blocking criteria.

- **Completion and timeouts:** tell workers when to call `kanban complete`; tune task/tool timeouts for long work; do not let a finished task remain blocked merely because a model defaults to human review.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
