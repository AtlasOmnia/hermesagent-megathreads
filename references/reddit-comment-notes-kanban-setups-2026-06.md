# Reddit Comment Notes — Kanban Setups Megathread — Hermes Agent (June 2026)

Original thread: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/

Captured for provenance during the July 16, 2026 migration. Classification is editorial triage, not independent verification. Promotional, reputational, pricing, benchmark, version, and availability claims require primary-source checks before entering the canonical guide.

Praise, jokes, GIFs, removed/deleted bodies, bot reminders, and other non-substantive comments are intentionally omitted.

### u/Jonathan_Rivera — Correction or contradiction

- Score at capture: 13
- Comment: # Added Bonus, What would Andrej Karpathy say about how to best utilize this setup. &gt;&gt;&gt;&gt; I don't think OP wrote this himself.. # Kanban, Profiles, and the Agent Loop — A Philosophy Check (Sticky) **A companion post to the Kanban Setups Megathread** There's a lot of energy around multi-agent setups right now. Six-profile pipelines. Orchestrators that decompose tasks. Fleet workers running overnight. It's exciting, and the megathread documents it well. But it's worth checking in with a simpler frame. This isn't a critique of Kanban — it's a reminder of what the agent loop actually is, and what happens when you abstract before you understand. --- ## The core loop is the real architecture Andrej Karpathy has been saying this for years, and it maps perfectly onto Hermes: &gt; The agent loop — **think → act → observe → repeat** — is the only architecture that matters. Everything else is papering over how well you do that loop. Before you add a second profile, ask: does your first agent complete one task reliably? Does it call the right tools, hand off cleanly, and stop when done? If the answer is "mostly" or "it depends on the model," another profile won't fix that. It will just make the failure more expensive because now two agents are failing in sequence. The dispatcher + Kanban worker lifecycle *is* the agent loop at the system level. You're not escaping the loop by adding profiles — you're just distributing it. That's fine when you've outgrown one agent. It's premature when you haven't. --- ## Every profile pays cold-start cost Each dispatcher spawn is a fresh process with a cold context window. The worker reads its task, loads parent handoffs, loads comments, loads skill files — all before it does any actual work. On a local model, that's seconds of overhead per spawn. On an API model, that's tokens burned before the first meaningful output. A single profile running 50 sequential turns costs less than 5 profiles running 10 turns each, because t … [excerpt intentionally limited to avoid republishing a large script or identity-specific configuration; use the source link for the full public comment]
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou0o32o/
### u/Sr_Alu — Correction or contradiction

- Score at capture: 3
- Comment: Some more tips based on my experience: Give a worker either a worker specific [AGENTS.md](http://AGENTS.md) in its working dir, or implement some "standard handoff/working rules" document that get auto attached to the workers session. \- You want to explicitly tell it to use kanban complete when the task is done, because some models seem to forget this and then the card gets blocked even tho the work is actually done. \- Some LLM (especially GPT5.5) also tends to always actively block a task when it done for "human review". if you dont want that, tell it explicitly to not do this. \- You want to drastically increase all relevant Kanban/Hermes related Timeouts, that will reduce the card failure rate by prolly 90% lol \- If the worker is supposed to use an MCP, you also might want to increase the MCP tool call failure limit (default is 3) \- Regarding retries, its a good idea to let another model do the retry instead of the same one. Depending on what you do, many blockers might actually occur because a model downright refuses the task (ie for cybersec or NSFW reasons)
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou56gtf/
### u/sbussiso_dube — Addition or operator report

- Score at capture: 2
- Comment: Thanks for all the info!
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou72gfg/
### u/Proparser — Addition or operator report

- Score at capture: 1
- Comment: Its not cheap, with cloud api llm?
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou1wck8/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: I would imagine that your user. md is very serious while mine is literally modeled after iron man's AI assistant lol.
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou25cbd/
### u/Gnillort123 — Addition or operator report

- Score at capture: 1
- Comment: Is there such a thing in openclaw?
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou387dp/
### u/rittatewa — Addition or operator report

- Score at capture: 1
- Comment: Same pain here from the Hermes setup angle: once Claude Code, Codex, Cursor, and n8n all share one upstream key, MCP tool wiring stops being just config and becomes an attribution problem. The hardening pattern I keep landing on is: do not give each runner the real OpenAI / Anthropic / GitHub key. Give each runner its own broker key, then let the broker inject the real downstream credential only on the proxy path. I've been working on an open-source version of that idea we're calling NyxID. The useful bit is not magic auth; it is the separation. In the model layer, `UserService` is the thing the agent is allowed to call, while `UserApiKey` is the encrypted downstream credential. In the proxy path, NyxID resolves the service, checks whether the agent key is scoped to it, decrypts the selected downstream credential, then the credential injection switch in `proxy_service.rs` applies it as bearer auth, custom header, query param, basic auth, etc. The part that mattered most for agent workflows was per-agent binding. Claude Code and n8n can both call the same logical `openai` service, but NyxID can inject different upstream keys for each agent. If the n8n workflow goes sideways, I can revoke that agent key or binding without breaking Claude Code, and logs can carry `X-NyxID-Agent-Id` for attribution. So my practical advice is: treat MCP/tool credentials as a routing boundary, not just env vars copied into every runner. Code is here: https://github.com/ChronoAIProject/NyxID
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/oua0205/
### u/Page_Specialist — Addition or operator report

- Score at capture: 0
- Comment: precisamos disso no App desktop!!!
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou0upx2/
### u/gigieazi — Addition or operator report

- Score at capture: 0
- Comment: LOVE kanban
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou0z648/
### u/riceinmybelly — Addition or operator report

- Score at capture: 1
- Comment: Hehe saw your post, wanted to link you but you found it on your own
- Source: https://www.reddit.com/r/hermesagent/comments/1ugkihk/kanban_setups_megathread_hermes_agent_june_2026/ou2fkdy/
