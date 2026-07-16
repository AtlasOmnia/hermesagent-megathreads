# Reddit Comment Notes — Multi-Agent & Profiles Megathread — Hermes Agent (June 2026)

Original thread: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/

Captured for provenance during the July 16, 2026 migration. Classification is editorial triage, not independent verification. Promotional, reputational, pricing, benchmark, version, and availability claims require primary-source checks before entering the canonical guide.

Praise, jokes, GIFs, removed/deleted bodies, bot reminders, and other non-substantive comments are intentionally omitted.

### u/riceinmybelly — Addition or operator report

- Score at capture: 8
- Comment: Ooh I’m feeding this into my setup right meow
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5301g/
### u/Nicolinux — Addition or operator report

- Score at capture: 3
- Comment: How would you use that with two distinct people? Create a profile for each? Or is it better to deploy two hermes agent instances?
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5dkce/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 2
- Comment: I’m doing this for the wife soon but I think it’s going to be just a profile.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5huiw/
### u/Nicolinux — Addition or operator report

- Score at capture: 1
- Comment: Yes same here. The problem I see is that profiles do not isolate the agents completely. I don't know what unexpected actions the other profile might perform which might jeopardize the entire installation (more than it does anyway...)
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot7dm4i/
### u/flairtestuser123 — Addition or operator report

- Score at capture: 2
- Comment: Do you have the delegate/kanban headings swapped on Part2 table?
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5cmld/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: Go ahead and quote the part and ill check it.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5d7dx/
### u/flairtestuser123 — Addition or operator report

- Score at capture: 2
- Comment: Why Kanban beats delegate_task for durable work: delegate_task Kanban Durability Lost if parent is interrupted Human in loop Not supported Multiple agents per task One subagent Audit trail Lost on compression Resumability None — failed = failed
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5jlj9/
### u/Jonathan_Rivera — Correction or contradiction

- Score at capture: 1
- Comment: Yep, good catch, it’s not swapped so much as missing the left “Criterion” column, which made Reddit render the delegate\_task limitations under Kanban. I’ll fix that table. Thanks for catching it.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5kuhw/
### u/Every-Equipment-3795 — Addition or operator report

- Score at capture: 1
- Comment: I was about to ask the same but you beat me to it
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5mir4/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: For some reason when put everything in markdown, It would not let me post unless I converted back into rich text.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5n43a/
### u/Every-Equipment-3795 — Addition or operator report

- Score at capture: 1
- Comment: That's frustrating. Regardless of that mix up - Thank you for this post! It's an excellent guide and I've been searching for information like this. I should've led with that
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot600sh/
### u/Paradigmind — Addition or operator report

- Score at capture: 2
- Comment: Are you using Hermes Desktop? Because for me the bot tokens are not isolated in the GUI.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5l9mu/
### u/arunsampath — Addition or operator report

- Score at capture: 2
- Comment: Same
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/oth88mn/
### u/xeeff — Addition or operator report

- Score at capture: 2
- Comment: this might be your longest post yet 😆 awesome guides like always
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/otaljt9/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: No, there's 1 where I had to post the rest in the comments 2x and sticky it lol
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ottzxc7/
### u/djseto — Correction or contradiction

- Score at capture: 2
- Comment: Lot to digest here. So...what I deally want is one agent I talk to that is more conversational/generic. If during that conversation, the agent decides it needs to write code, I want it to talk to/spawn another agent who's my coder agent to delegate the work. Which is the best way to build this? For example, earlier today i asked Hermes to use Vane (running in Docker) to go research some stuff on the web. It didnt work because of a config change. After lots of back and forth, we found the issue in a python script so it wanted to fix the script. That's when things went sideways and I wonder if it was because I didn't have a coding defined agent (different temp etc.) do take on that task. Am I making any sense? Is that profiles? Is that delegate task? I dont want to tell the primary agent who to delegate to. I want it to decide based on the task (e.g. if we have to fix code, use the code agent).
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ou1z5v6/
### u/Jonathan_Rivera — Correction or contradiction

- Score at capture: 1
- Comment: Your problem isn't architecture. It's context. A separate agent with different temperature wouldn't have saved you. The model didn't have the right context: what broke, what to preserve, what the expected behavior was. LLMs are like people. You manage them the way you'd manage a person: give them clear instructions, the right context, and the right tools. You don't hire a separate "typing specialist" with a different personality for every subtask. You don't need a separate coding profile with different temperature. Hermes already has delegate\_task built in. The primary agent can spawn subagents with coding tools when it decides the task calls for it. This is exactly how Claude Code subagents work, the main agent reads the subagent's description and decides when to delegate. What you actually need: 1. One conversational agent 2. A system prompt rule: "when a task requires nontrivial code changes, spawn a subagent with terminal + file tools" 3. The subagent gets isolated context, does the work, returns the result 4. If things go wrong, fix the context you're passing to the subagent, not the architecture You're 90% there and overthinking the last 10%. The config change debugging saga wasn't an architecture gap, it was the model working with incomplete information about what had changed and what needed preserving. Better prompts, not more agents. Now I would consider having a verifier/critic pass on important outputs. or as they would call it a loop. Generate with one session, review with another. That's a more of a quality gate, not a different-temperature "coder personality." Same model, fresh eyes, strict critique prompt.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ou23hwc/
### u/djseto — Correction or contradiction

- Score at capture: 1
- Comment: I’m running qwen3.6 35b a3b using Ollama . The mega thread you have on Mac says for coding work the temp should be .6. For conversation etc it should be .8-1. How does Hermes know to spawn a coding agent with the right temp for coding work? To be clear, I’m not a developer so I was letting Hermes fix the script because it decided to. It figured out the issue and the decided to fix it. It just got stuck in some loop and eventually knew it was going down a rabbit hole and stopped itself and tried to start over.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ou273e8/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 2
- Comment: That is the rough part of local that I run into. Ideally you would run a 2nd model but on a mac you probably don't have the space. Consider using deepseek API for delegated tasks, $10 would last a while if your not using it frequently.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ou28iq8/
### u/djseto — Addition or operator report

- Score at capture: 1
- Comment: Then how do I tell Hermes to use a different model for coding agent? Is that the profile method?
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ou28n9e/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 2
- Comment: You can assign a model to subagents or you can create another profile.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ou2a40x/
### u/blackhawk00001 — Correction or contradiction

- Score at capture: 1
- Comment: Nice, I've been putting together a coordination framework with memory for pulling together various agent frameworks. I like this approach. Does your framework allow for configuring various local gpu inference endpoints for profiles/agents? I've taken a break from the other project to play with hermes and began working on extending the base subagent provider system to support routing to various local servers based on round-robin or semantically matched for purpose with thread queueing for limited concurrency. It's working but I'm ironing out bugs while getting a feel for the codebase.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot55yhb/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 3
- Comment: The megathreads are more scraping community questions and pairing with answers along with data collection/research on the topics. I use these just like you to see what the next step would be. My workshop posts are things I have already implemented.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot56n5c/
### u/leggomycraiggo — Addition or operator report

- Score at capture: 1
- Comment: Any advice for using this setup when you want to have Hermes call different models for different purposes? For instance, in the "personality split" setup, is it possible to have the one Hermes agent that uses a dense LLM for coding tasks while having a more nimble MOE for testing?
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot5pj4e/
### u/banditjackpotty — Addition or operator report

- Score at capture: 1
- Comment: I haven't done it but it seems possible in my set up. You just set the other profile with another model. For coding, I have mine set up to pass off to Claude code instead
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot9mc70/
### u/russjr08 — Addition or operator report

- Score at capture: 1
- Comment: Yeah, profiles can be set to run on different models - my "general" one for example uses DS v4 Flash, while my pair programming one is setup for GPT 5.5 There's a few options here that I'd investigate for this sort of thing. I believe the `delegate_task` tool that the agents have by default just make a temporary "clone" of itself, but you could probably have your agent instead use the `hermes --profile name --oneshot Prompt For Task Goes Here` command to do the delegation instead. That's hermes' equivalent of claude -p that some people do. Alternatively if you don't want to have a separate profile, you can still have the agent use that oneshot command with a model/provider override instead like `hermes --provider openai --model gpt-5.5 --oneshot PROMPT` which would spawn a temporary instance of the same agent for the new task, but with a different model loaded. Another option is to just [override the model that the delegation tool creates](https://hermes-agent.nousresearch.com/docs/user-guide/features/delegation/#model-override) via the config file, but that would apply to all delegations. In that case you could have your agent's main model be the lighter one, and then just instruct it for all coding tasks to be delegated out (and you'd set your dense model as the delegation model). Edit: Oh and of course, Kanban can probably be used for this too.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/oticlsy/
### u/leggomycraiggo — Addition or operator report

- Score at capture: 1
- Comment: Brilliant, thank you for the quick walkthrough!
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/otino2k/
### u/jaybsuave — Addition or operator report

- Score at capture: 1
- Comment: think i will run this in the background on files that i consider finish, so like a constant code review on files and then allow a multi agent hermes worflow completely commit to code review after running multiple agents through opencode.. trying to figure out how to work this into my workflow
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ot981a1/
### u/dreamzzftw — Addition or operator report

- Score at capture: 1
- Comment: Is there any way of keeping configs between profiles on sync? I find that having to make sure the config of 9 different profiles can be tedious. The only thing I don’t want synced is the model used for each profile
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/ottzms5/
### u/Jonathan_Rivera — Addition or operator report

- Score at capture: 1
- Comment: I think you can probably prompt your hermes to map the locations lines in each config and create a script that can change it or keep in sync.
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/otu0tbz/
### u/dreamzzftw — Addition or operator report

- Score at capture: 1
- Comment: Great idea, thank you
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/otucsup/
### u/errantekarmico — Addition or operator report

- Score at capture: 1
- Comment: You can create symlinks to the main configuration files
- Source: https://www.reddit.com/r/hermesagent/comments/1ucmvi8/multiagent_profiles_megathread_hermes_agent_june/otua2sg/
