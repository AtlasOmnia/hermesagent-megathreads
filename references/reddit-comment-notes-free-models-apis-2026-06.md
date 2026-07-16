# Reddit Comment Notes — Free Models and APIs Megathread

Source: https://www.reddit.com/r/hermesagent/comments/1uj9nkn/free_models_apis_for_hermes_agent_megathread_june/


This file archives comments captured from the RSS feed so future maintainers can see what changed.

This archive excludes Jonathan's own follow-up comments so the reference file focuses on community feedback and suggestions.
## 1. /u/akgo

- Link:


Who is actually able to use these free models.

The many posts about Nvidia and free mini Max and deep seek Yesterday I got API key from Nvidia and it literally run out like 5 minutes. I ask you basic questions from minimax and its gone. So what is the use of API keys that are free. If we keep on switching all the long or am I making some mistakes.

Why would anyone give you such free quota.?

## 3. /u/akgo

- Link:


Makes sense. Deploy it for small tasks is a good idea but again it takes a lot of time to check everything in blah blah blah. How do you deal with that do you have a system in place?

Aisi so many people on forum using these free API But it's such a nightmare to keep on juggling between things

For example I have been using anti gravity and then I move to hermies with deepseek v4f but now moving to minimax as ds is giving shitty output.

But not to adjust to it and how it behaves I have to think again and understand the models behaviour. I don't know how people are so dynamic to use x for that, y for this and so on .

## 5. /u/tomByrer

- Link:


This should be in a GitHub repo or something.

## 6. /u/xhp-eth

- Link:


I agree with this one. So everyone can be a collaborator to update the information.

## 8. /u/tomByrer

- Link:


Mostly because other people can help edit, & you can approve said edits or refuse.
 Also there are many tools to help automate Github, such as summarizing diffs (which you can post here).
 + it is another promotion channel.

## 10. /u/fleperson

- Link:


- Install Obsidian (visual editing Markdown files)

- Create an empty GH repo

- Install Git plugin from Obsidian community plugins so you can sync/push to the repo whenver you have a new one ready.

- Install some AI Asistant Obsidian Plugin, so you can hook your AI's inside it in case you want to use it to edit / ajust your files too.

GG

## 11. /u/tomByrer

- Link:


Any git client (including the CLI) can sync between Obsidian & Github.

I personally start my repo in
Github.com
 with a blank README, clone in cli, then add & edit from there.

## 12. /u/fleperson

- Link:


I know, I just suggested a super easy path for OP as he stated he is not much famliar with GH. Doing with plugins inside Obsidian might be easier to start fiddling with it.

## 13. /u/tomByrer

- Link:


I use Obsidian, but using with plugins it is kinda advanced. I still don't know how plugins do NOT get cloned from my desktop to mobile versions.

BTW, this is a HERMES AGENT sub; folks should be automating everything, not using plugins ;)

## 14. /u/chanc2

- Link:


I’ve used OpenRouter:free in the past with OpenClaw. Found it to be unreliable and slow. Moved to DeepSeek v4 using Deepseek’s API with Hermes and it’s been working great.

## 15. /u/chesco11

- Link:


oh I wonder if there's a difference with using Deepseek via openrouter api..

## 16. /u/chanc2

- Link:


I went direct to DS API and I’m really happy with the performance. No delay in the responses. I didn’t try DA via OpenRouter. I wanted to tap into the super low token pricing that DS is offering.

## 17. /u/chesco11

- Link:


thank you! that makes sense!

## 18. /u/moreoronce

- Link:


"SiliconFlow deserves a spot on this list — free API access to Qwen3 models including the VL-32B variant with native vision input. I've been running it as my image analysis provider in Hermes for a while and tool-use works well out of the box.

One gotcha worth sharing: if you're wiring up a non-standard provider for vision in Hermes, check your
agent.image_input_mode
 setting. I had a case where image inputs were silently degrading to text because my main model wasn't in the models .dev cache Hermes uses to detect vision capability. The vision model was there, tokens were being spent, but it never actually
saw
 the image. Setting
image_input_mode: native
 fixed it immediately.

Also — if your agent tasks touch any Chinese/CJK content, Qwen3 on SiliconFlow handles it noticeably better than most OpenRouter free models. Worth having in your failover chain for that reason alone.

## 19. /u/Kamalcr77

- Link:


Wait, is deepseek free on open router? Are you sure?

## 21. /u/Kamalcr77

- Link:


It's not available anymore... Please verify. Check openrouter/free. It's not listed there.

## 23. /u/impoze

- Link:


Also tried but says not available free anymore

## 25. /u/impoze

- Link:


I think that ends soon though, almost at 30 days.

I've been using openrouter/owl -alpha and nvidia/nemotron-3

## 27. /u/Draunzr

- Link:


What happens after that ? They'll remove the free tier completely or give us a even dumber model ?

## 29. /u/TourSignificant7065

- Link:


Owl alpha is retiring soon so be wary about that

## 31. /u/spacenglish

- Link:


Graduated from free to paid.

## 32. /u/carbolymer

- Link:


Q: What free APIs exist outside of OpenRouter? A: Google AI Studio,

I'm constantly getting 429 for Gemini 2.5 Flash Lite with 0 usage, so how it is free?

https://platform.deepseek.com/

how do you add an api key for free tier?

## 33. /u/brandeded

- Link:


I use groq, but leverage litellm locally hosted for routing. Still working this out.

## 34. /u/knowoneknows

- Link:


How do yall handle the data retention issue with free models? Are you okay allowing the collection of any and all data you're working with?

## 35. /u/jkoehler11

- Link:


I've been using deepseek-v4-pro under the free opencode.ai go tier and I have yet to hit any of the limits. I have openrouter/free and local ollama for backup but haven't needed it.

## 36. /u/pumpkin_biscuits1

- Link:


Sadly Owl-Alpha disappeared overnight. It is going to be a paid model soon.

## 38. /u/Draunzr

- Link:


What about stepfun step 3.7 flash. Been using, it does the tasks mostly but gives gibberish replies sometimes like I haven't asked it anything related to docker at all and it'll give a response explaining docker on its own. Does someone know how stepfun compared to like owl alpha for gemma 4
