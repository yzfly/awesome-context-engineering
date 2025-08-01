Title: How to Fix Your Context

URL Source: https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html

Published Time: 2025-06-26T08:23:00-07:00

Markdown Content:
![Image 1](https://www.dbreunig.com/img/overload.jpg)

### Mitigating & Avoiding Context Failures

Following up on our earlier post, “[How Long Contexts Fail](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)”, let’s run through the ways we can mitigate or avoid these failures entirely.

But before we do, let’s briefly recap some of the ways long contexts can fail:

*   **Context Poisoning:** When a hallucination or other error makes it into the context, where it is repeatedly referenced.
*   **Context Distraction:** When a context grows so long that the model over-focuses on the context, neglecting what it learned during training.
*   **Context Confusion:** When superfluous information in the context is used by the model to generate a low-quality response.
*   **Context Clash:** When you accrue new information and tools in your context that conflicts with other information in the prompt.

Everything here is about information management. Everything in the context influences the response. We’re back to the old programming adage of, “[Garbage in, garbage out](https://en.wikipedia.org/wiki/Garbage_in,_garbage_out).” Thankfully, there’s plenty of options for dealing with the issues above.

Context Management Tactics
--------------------------

*   [**RAG:** Selectively adding relevant information to help the LLM generate a better response](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#rag)
*   [**Tool Loadout:** Selecting only relevant tool definitions to add to your context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#tool-loadout)
*   [**Context Quarantine:** Isolating contexts in their own dedicated threads](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-quarantine)
*   [**Context Pruning:** Removing irrelevant or otherwise unneeded information from the context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-pruning)
*   [**Context Summarization:** Boiling down an accrued context into a condensed summary](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-summarization)
*   [**Context Offloading:** Storing information outside the LLM's context, usually via a tool that stores and manages the data](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-offloading)

* * *

![Image 2: A stack of papers.](https://www.dbreunig.com/img/rag.png)

### RAG

_Retrieval-Augmented Generation (RAG) is the act of selectively adding relevant information to help the LLM generate a better response._

So much has been written about RAG, we’re not going to cover it today beyond saying: it’s very much alive.

Every time a model ups the context window ante, a new “RAG is Dead” debate is born. The last significant event was when Llama 4 Scout landed with a _10 million token window_. At that size it’s _really_ tempting to think, “Screw it, throw it all in,” and call it a day.

But, as we covered last time: if you treat your context like a junk drawer, the junk will [influence your response](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-confusion). If you want to learn more, here’s a [new course that looks great](https://maven.com/p/569540/i-don-t-use-rag-i-just-retrieve-documents).

![Image 3: A selection of guns from the videogame Destiny](https://www.dbreunig.com/img/tool_loadout.png)

### Tool Loadout

_Tool Loadout is the act of selecting only relevant tool definitions to add to your context._

The term “loadout” is a gaming term that refers to the specific combination of abilities, weapons, and equipment you select before a level, match, or round. Usually, your loadout is tailored to the context – the character, the level, the rest of your team’s makeup, and your own skillset.

Here, we’re borrowing the term to describe selecting the most relevant tools for a given task.

Perhaps the simplest way to select tools is to apply RAG to your tool descriptions. This is exactly what Tiantian Gan and Qiyao Sun did, which they detail in their paper, “[RAG MCP](https://arxiv.org/abs/2505.03275).” By storing their tool descriptions in a vector database, they’re able to select the most relevant tools given an input prompt.

When prompting DeepSeek-v3, the team found that selecting the the right tools becomes critical when you have more than 30 tools. Above 30, the descriptions of the tools begin to overlap, creating confusion. Beyond _100 tools_, the model was virtually guaranteed to fail their test. Using RAG techniques to select less than 30 tools yielded dramatically shorter prompts and resulted in as much as 3x better tool selection accuracy.

For smaller models, the problems begin long before we hit 30 tools. One paper we touched on last post, “[Less is More](https://arxiv.org/abs/2411.15399),” demonstrated that Llama 3.1 8b fails a benchmark when given 46 tools, but succeeds when given only 19 tools. The issue is context confusion, _not_ context window limitaions.

To address this issue, the team behind “Less is More” developed a way to dynamically select tools using a LLM-powered tool recommender. The LLM was prompted to reason about, “number and type of tools it ‘believes’ it requires to answer the user’s query.” This output was then semantically searched (tool RAG, again) to determine the final loadout. They tested this method with the [Berkeley Function Calling Leaderboard](https://gorilla.cs.berkeley.edu/leaderboard.html), finding Llama 3.1 8b performance improved by 44%.

The “Less is More” paper notes two other benefits to smaller contexts: reduced power consumption and speed, crucial metrics when operating at the edge (meaning, running an LLM on your phone or PC, not on a specialized server). Even when their dynamic tool selection method _failed_ to improve a model’s result, the power savings and speed gains were worth the effort, yielding savings of 18% and 77%, respectively.

Thankfully, most agents have smaller surface areas that only require a few, hand-curated tools. But if the breadth of functions or the amount of integrations needs to expand, always consider your loadout.

![Image 4: A belljar](https://www.dbreunig.com/img/context_quarantine.png)

### Context Quarantine

_Context Quarantine is the act of isolating contexts in their own dedicated threads, each used separately by one or more LLMs._

We see better results when our contexts aren’t too long and don’t sport irrelevant content. One way to achieve this is to break our tasks up into smaller, isolated jobs – each with their own context.

There are [many](https://arxiv.org/abs/2402.14207)[examples](https://www.microsoft.com/en-us/research/articles/magentic-one-a-generalist-multi-agent-system-for-solving-complex-tasks/) of this tactic, but an accessible write up of this strategy is Anthropic’s [blog post detailing their multi-agent research system](https://www.anthropic.com/engineering/built-multi-agent-research-system). They write:

> The essence of search is compression: distilling insights from a vast corpus. Subagents facilitate compression by operating in parallel with their own context windows, exploring different aspects of the question simultaneously before condensing the most important tokens for the lead research agent. Each subagent also provides separation of concerns—distinct tools, prompts, and exploration trajectories—which reduces path dependency and enables thorough, independent investigations.

Research lends itself to this design pattern. When given a question, several subquestions or areas of exploration can be identified and separately prompted using multiple agents. This not only speeds up the information gathering and distillation (if there’s compute available), but it keeps each context from accruing too much information or information not relevant to a given prompt, delivering higher quality results:

> Our internal evaluations show that multi-agent research systems excel especially for breadth-first queries that involve pursuing multiple independent directions simultaneously. We found that a multi-agent system with Claude Opus 4 as the lead agent and Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by 90.2% on our internal research eval. For example, when asked to identify all the board members of the companies in the Information Technology S&P 500, the multi-agent system found the correct answers by decomposing this into tasks for subagents, while the single agent system failed to find the answer with slow, sequential searches.

This approach also helps with tool loadouts, as the agent designer can create several agent archetypes with their own dedicated loadout and instructions for how to utilize each tool.

The challenge for agent builders, then, is to find opportunities for isolated tasks to spin out onto separate threads. Problems that require context-sharing among multiple agents aren’t particularly suited to this tactic.

If your agent’s domain is at all suited to parallelization, be sure to [read the whole Anthropic write up](https://www.anthropic.com/engineering/built-multi-agent-research-system). It’s excellent.

![Image 5: Pruning shears](https://www.dbreunig.com/img/context_pruning.png)

### Context Pruning

_Context Pruning is the act of removing irrelevant or otherwise unneeded information from the context._

Agents accrue context as they fire off tools and assemble documents. At times, it’s worth pausing to assess what’s been assembled and remove the cruft. This could be something you task your main LLM with, or you could design a separate LLM-powered tool to review and edit the context. Or you could choose something more tailored to the pruning task.

Context pruning has a (relatively) long history, as context lengths were a more problematic bottleneck in the natural language processing (NLP) field, prior to ChatGPT. A current pruning method, building on this history, is [Provence](https://arxiv.org/abs/2501.16214), “an efficient and robust context pruner for Question Answering.”

Provence is fast, accurate, simple to use, and relatively small – only 1.75 GB. You can call it in a few lines, like so:

```
from transformers import AutoModel

provence = AutoModel.from_pretrained("naver/provence-reranker-debertav3-v1", trust_remote_code=True)

# Read in a markdown version of the Wikipedia entry for Alameda, CA
with open('alameda_wiki.md', 'r', encoding='utf-8') as f:
    alameda_wiki = f.read()

# Prune the article, given a question
question = 'What are my options for leaving Alameda?'
provence_output = provence.process(question, alameda_wiki)
```

Provence edited down the article, cutting 95% of the content, leaving me with only [this relevant subset](https://gist.github.com/dbreunig/b3bdd9eb34bc264574954b2b954ebe83). It nailed it.

One could employ Provence or a similar function to cull down documents or the entire context. Further, this pattern is a strong argument for maintaining a _structured_[1](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#fn:structure) version of your context in a dictionary or other form, from which you assemble a compiled string prior to every LLM call. This structure would come in handy when pruning, allowing you to ensure the main instructions and goals are preserved while the document or history sections can be pruned or summarized.

![Image 6: A duck press](https://www.dbreunig.com/img/context_summarization.png)

### Context Summarization

_Context Summarization is the act of boiling down an accrued context into a condensed summary._

Context Summarization first appeared as a tool for dealing with smaller context windows. As your chat session came close to exceeding the maximum context length, a summary would be generated and a new thread would begin. Chatbot users did this manually, in ChatGPT or Claude, asking the bot to generate a short recap which would then be pasted into a new session.

However, as context windows increased, agent builders discovered there’s benefits to summarization beyond staying within the total context limit. As the context grows, it becomes distracting and causes the model to rely less on what it learned during training. We called this [Context Distraction](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-distraction). The team behind the Pokémon-playing Gemini agent discovered anything beyond 100,000 tokens triggered this behavior:

> While Gemini 2.5 Pro supports 1M+ token context, making effective use of it for agents presents a new research frontier. In this agentic setup, it was observed that as the context grew significantly beyond 100k tokens, the agent showed a tendency toward favoring repeating actions from its vast history rather than synthesizing novel plans. This phenomenon, albeit anecdotal, highlights an important distinction between long-context for retrieval and long-context for multi-step, generative reasoning.

Summarizing your context is easy to do, but hard to perfect for any given agent. Knowing what information should be preserved, and detailing that to an LLM-powered compression step, is critical for agent builders. It’s worth breaking out this function as it’s own LLM-powered stage or app, which allows you to collect evaluation data that can inform and optimize this task directly.

![Image 7: A banker's box](https://www.dbreunig.com/img/context_offload.png)

### Context Offloading

_Context Offloading is the act of storing information outside the LLM’s context, usually via a tool that stores and manages the data._

This might be my favorite tactic, if only because it’s so _simple_ you don’t believe it will work.

Again, [Anthropic has a good write up of the technique](https://www.anthropic.com/engineering/claude-think-tool), which details their “think” tool, which is basically a scratchpad:

> With the “think” tool, we’re giving Claude the ability to include an additional thinking step—complete with its own designated space—as part of getting to its final answer… This is particularly helpful when performing long chains of tool calls or in long multi-step conversations with the user.

I really appreciate the research and other writing Anthropic publishes, but I’m not a fan of this tool’s name. If this tool were called `scratchpad`, you’d know its function _immediately_. It’s a place for the model to write down notes that don’t cloud its context, that are available for later reference. The name “think” clashes with “[extended thinking](https://www.anthropic.com/news/visible-extended-thinking)” and needlessly anthropomorphizes the model…but I digress.

Having a space to log notes and progress _works_. Anthropic shows pairing the “think” tool with a domain-specific prompt (which you’d do anyway in an agent) yields significant gains, up to a 54% improvement against a benchmark for specialized agents.

Anthropic identified three scenarios where the context offloading pattern is useful:

> 1.   Tool output analysis. When Claude needs to carefully process the output of previous tool calls before acting and might need to backtrack in its approach;
> 2.   Policy-heavy environments. When Claude needs to follow detailed guidelines and verify compliance; and
> 3.   Sequential decision making. When each action builds on previous ones and mistakes are costly (often found in multi-step domains).

* * *

Context management is usually the hardest part of building an agent. Programming the LLM to, as Karpathy says, “[pack the context windows just right](https://x.com/karpathy/status/1937902205765607626)”, smartly deploying tools, information, and regular context maintenance is _the_ job of the agent designer.

The key insight across all the above tactics is that _context is not free_. Every token in the context influences the model’s behavior, for better or worse. The massive context windows of modern LLMs are a powerful capability, but they’re not an excuse to be sloppy with information management.

As you build your next agent or optimize an existing one, ask yourself: Is everything in this context earning its keep? If not, you now have six ways to fix it.

* * *

1.   Hell, this entire list of tactics is a strong argument [you should program your contexts](https://www.dbreunig.com/2025/06/10/let-the-model-write-the-prompt.html).[↩](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#fnref:structure)
