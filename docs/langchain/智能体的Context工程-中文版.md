# 智能体的Context工程

原文链接：https://blog.langchain.com/context-engineering-for-agents/

发布时间：2025-07-02T15:52:42.000Z

### 摘要

智能体需要context来执行任务。Context工程是在智能体轨迹的每一步中，用恰当的信息填充context窗口的艺术和科学。在这篇文章中，我们通过回顾各种流行的智能体和论文，分解了一些常见的策略——**写入、选择、压缩和隔离**——用于context工程。然后我们解释LangGraph是如何设计来支持它们的！

**另外，请观看我们关于context工程的视频**[**这里**](https://youtu.be/4GiqzUHD5AA?ref=blog.langchain.com)**。**

![Image 1](https://blog.langchain.com/content/images/2025/07/image.png)

Context工程的一般类别

### Context工程

正如Andrej Karpathy所说，LLM就像一种[新型操作系统](https://www.youtube.com/watch?si=-aKY-x57ILAmWTdw&t=620&v=LCEmiRjPEtQ&feature=youtu.be&ref=blog.langchain.com)。LLM就像CPU，其[context窗口](https://docs.anthropic.com/en/docs/build-with-claude/context-windows?ref=blog.langchain.com)就像RAM，作为模型的工作内存。就像RAM一样，LLM context窗口的[容量](https://lilianweng.github.io/posts/2023-06-23-agent/?ref=blog.langchain.com)有限，无法处理各种context来源。正如操作系统策划适合CPU RAM的内容一样，我们可以考虑"context工程"发挥类似的作用。[Karpathy很好地总结了这一点](https://x.com/karpathy/status/1937902205765607626?ref=blog.langchain.com)：

> _[Context工程是]"...用恰当的信息为下一步填充context窗口的精妙艺术和科学。"_

![Image 2](https://blog.langchain.com/content/images/2025/07/image-1.png)

LLM应用中常用的context类型

在构建LLM应用时，我们需要管理哪些类型的context？Context工程作为一个[总括概念](https://x.com/dexhorthy/status/1933283008863482067?ref=blog.langchain.com)，适用于几种不同的context类型：

* **指令** – prompt、记忆、few-shot示例、工具描述等
* **知识** – 事实、记忆等
* **工具** – 工具调用的反馈

### 智能体的Context工程

今年，随着LLM在[推理](https://platform.openai.com/docs/guides/reasoning?api-mode=responses&ref=blog.langchain.com)和[工具调用](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)方面变得更好，对[智能体](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)的兴趣大幅增长。[智能体](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)交替进行[LLM调用和工具调用](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)，通常用于[长期运行的任务](https://blog.langchain.com/introducing-ambient-agents/)。智能体交替进行[LLM调用和工具调用](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)，使用工具反馈来决定下一步。

![Image 3](https://blog.langchain.com/content/images/2025/07/image-2.png)

智能体交替进行[LLM调用和](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)[工具调用](https://www.anthropic.com/engineering/building-effective-agents?ref=blog.langchain.com)，使用工具反馈来决定下一步

然而，长期运行的任务和工具调用的累积反馈意味着智能体通常使用大量token。这可能导致许多问题：可能[超过context窗口的大小](https://cognition.ai/blog/kevin-32b?ref=blog.langchain.com)，成本/延迟激增，或降低智能体性能。Drew Breunig[很好地概述了](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html?ref=blog.langchain.com)更长context可能导致性能问题的几种具体方式，包括：

* [Context中毒：当幻觉进入context时](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html?ref=blog.langchain.com#context-poisoning)
* [Context分心：当context压倒训练时](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html?ref=blog.langchain.com#context-distraction)
* [Context混乱：当多余的context影响响应时](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html?ref=blog.langchain.com#context-confusion)
* [Context冲突：当context的部分内容不一致时](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html?ref=blog.langchain.com#context-clash)

![Image 4](https://blog.langchain.com/content/images/2025/07/image-3.png)

工具调用的context在多个智能体轮次中累积

考虑到这一点，[Cognition](https://cognition.ai/blog/dont-build-multi-agents?ref=blog.langchain.com)指出了context工程的重要性：

> _"Context工程"...实际上是构建AI智能体的工程师的#1工作。_

[Anthropic](https://www.anthropic.com/engineering/built-multi-agent-research-system?ref=blog.langchain.com)也明确表达了这一点：

> _智能体经常进行跨越数百轮的对话，需要仔细的context管理策略。_

那么，人们今天是如何应对这一挑战的呢？我们将智能体context工程的常见策略分为四个类别——**写入、选择、压缩和隔离**——并通过回顾一些流行的智能体产品和论文给出每种策略的示例。然后我们解释LangGraph是如何设计来支持它们的！

![Image 5](https://blog.langchain.com/content/images/2025/07/image-4.png)

Context工程的一般类别

### 写入Context

_写入context意味着将其保存在context窗口之外，以帮助智能体执行任务。_

**Scratchpad**

当人类解决任务时，我们会做笔记并记住未来相关任务的事情。智能体也在获得这些能力！通过"[scratchpad](https://www.anthropic.com/engineering/claude-think-tool?ref=blog.langchain.com)"做笔记是在智能体执行任务时持久化信息的一种方法。其想法是将信息保存在context窗口之外，以便智能体可以使用。[Anthropic的多智能体研究员](https://www.anthropic.com/engineering/built-multi-agent-research-system?ref=blog.langchain.com)说明了一个清晰的例子：

> _LeadResearcher首先思考方法并将其计划保存到Memory中以持久化context，因为如果context窗口超过200,000个token，它将被截断，保留计划很重要。_

Scratchpad可以通过几种不同的方式实现。它们可以是一个[工具调用](https://www.anthropic.com/engineering/claude-think-tool?ref=blog.langchain.com)，简单地[写入文件](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem?ref=blog.langchain.com)。它们也可以是运行时[状态对象](https://langchain-ai.github.io/langgraph/concepts/low_level/?ref=blog.langchain.com#state)中的一个字段，在会话期间持久存在。无论哪种情况，scratchpad都让智能体保存有用的信息来帮助它们完成任务。

**记忆**

Scratchpad帮助智能体在给定会话（或[线程](https://langchain-ai.github.io/langgraph/concepts/persistence/?ref=blog.langchain.com#threads)）内解决任务，但有时智能体受益于跨_多个_会话记住事情！[Reflexion](https://arxiv.org/abs/2303.11366?ref=blog.langchain.com)引入了在每个智能体轮次后进行反思并重新使用这些自生成记忆的想法。[生成式智能体](https://ar5iv.labs.arxiv.org/html/2304.03442?ref=blog.langchain.com)从过去智能体反馈的集合中定期合成记忆。

![Image 6](https://blog.langchain.com/content/images/2025/07/image-5.png)

可以使用LLM来更新或创建记忆

这些概念进入了流行产品，如[ChatGPT](https://help.openai.com/en/articles/8590148-memory-faq?ref=blog.langchain.com)、[Cursor](https://forum.cursor.com/t/0-51-memories-feature/98509?ref=blog.langchain.com)和[Windsurf](https://docs.windsurf.com/windsurf/cascade/memories?ref=blog.langchain.com)，它们都有基于用户-智能体交互自动生成可跨会话持久存在的长期记忆的机制。

### 选择Context

_选择context意味着将其拉入context窗口以帮助智能体执行任务。_

**Scratchpad**

从scratchpad选择context的机制取决于scratchpad的实现方式。如果它是一个[工具](https://www.anthropic.com/engineering/claude-think-tool?ref=blog.langchain.com)，那么智能体可以通过进行工具调用来简单地读取它。如果它是智能体运行时状态的一部分，那么开发者可以选择在每一步向智能体暴露状态的哪些部分。这为在后续轮次中向LLM暴露scratchpad context提供了细粒度的控制级别。

**记忆**

如果智能体有保存记忆的能力，它们也需要选择与正在执行的任务相关的记忆的能力。这可能有几个原因很有用。智能体可能选择few-shot示例（[情节性](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#memory-types)[记忆](https://arxiv.org/pdf/2309.02427?ref=blog.langchain.com)）作为期望行为的示例，指令（[程序性](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#memory-types)[记忆](https://arxiv.org/pdf/2309.02427?ref=blog.langchain.com)）来引导行为，或事实（[语义](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#memory-types)[记忆](https://arxiv.org/pdf/2309.02427?ref=blog.langchain.com)）用于任务相关的context。

![Image 7](https://blog.langchain.com/content/images/2025/07/image-6.png)

一个挑战是确保选择相关的记忆。一些流行的智能体简单地使用一组狭窄的文件，这些文件_总是_被拉入context。例如，许多代码智能体使用特定文件来保存指令（"程序性"记忆）或在某些情况下保存示例（"情节性"记忆）。Claude Code使用[`CLAUDE.md`](http://claude.md/?ref=blog.langchain.com)。[Cursor](https://docs.cursor.com/context/rules?ref=blog.langchain.com)和[Windsurf](https://windsurf.com/editor/directory?ref=blog.langchain.com)使用规则文件。

但是，如果智能体存储更大的事实和/或关系[集合](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#collection)（例如，[语义](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#memory-types)记忆），选择就更困难了。[ChatGPT](https://help.openai.com/en/articles/8590148-memory-faq?ref=blog.langchain.com)是一个流行产品的好例子，它存储并从大量用户特定记忆中进行选择。

Embedding和/或[知识](https://arxiv.org/html/2501.13956v1?ref=blog.langchain.com#:~:text=In%20Zep%2C%20memory%20is%20powered,subgraph%2C%20and%20a%20community%20subgraph)[图](https://neo4j.com/blog/developer/graphiti-knowledge-graph-memory/?ref=blog.langchain.com#:~:text=changes%20since%20updates%20can%20trigger,and%20holistic%20memory%20for%20agentic)用于记忆索引通常用于协助选择。尽管如此，记忆选择仍然具有挑战性。在AIEngineer World's Fair上，[Simon Willison分享了](https://simonwillison.net/2025/Jun/6/six-months-in-llms/?ref=blog.langchain.com)一个选择出错的例子：ChatGPT从记忆中获取了他的位置并意外地将其注入到请求的图像中。这种意外或不期望的记忆检索可能让一些用户感觉context窗口"_不再属于他们_"！

**工具**

智能体使用工具，但如果提供太多工具可能会过载。这通常是因为工具描述重叠，导致模型对使用哪个工具感到困惑。一种方法是[对工具描述应用RAG（检索增强生成）](https://arxiv.org/abs/2410.14594?ref=blog.langchain.com)，以便仅获取任务最相关的工具。一些[最近的论文](https://arxiv.org/abs/2505.03275?ref=blog.langchain.com)表明这将工具选择准确性提高了3倍。

**知识**

[RAG](https://github.com/langchain-ai/rag-from-scratch?ref=blog.langchain.com)是一个丰富的主题，它[可能是一个核心的context工程挑战](https://x.com/_mohansolo/status/1899630246862966837?ref=blog.langchain.com)。代码智能体是大规模生产中RAG的一些最佳示例。来自Windsurf的Varun很好地捕捉了其中一些挑战：

> _索引代码 ≠ context检索...我们正在进行索引和embedding搜索...[使用]AST解析代码并沿着语义有意义的边界进行分块...随着代码库规模的增长，embedding搜索作为检索启发式变得不可靠...我们必须依赖grep/文件搜索、基于知识图的检索等技术的组合，以及...一个重新排序步骤，其中[context]按相关性顺序排序。_

### 压缩Context

_压缩context涉及仅保留执行任务所需的token。_

**Context摘要**

智能体交互可以跨越[数百轮](https://www.anthropic.com/engineering/built-multi-agent-research-system?ref=blog.langchain.com)并使用token密集的工具调用。摘要是管理这些挑战的一种常见方法。如果你使用过Claude Code，你已经看到了这一点的实际应用。Claude Code在你超过context窗口的95%后运行"[自动压缩](https://docs.anthropic.com/en/docs/claude-code/costs?ref=blog.langchain.com)"，它将摘要用户-智能体交互的完整轨迹。这种跨[智能体轨迹](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#manage-short-term-memory)的压缩类型可以使用各种策略，如[递归](https://arxiv.org/pdf/2308.15022?ref=blog.langchain.com#:~:text=the%20retrieved%20utterances%20capture%20the,based%203)或[分层](https://alignment.anthropic.com/2025/summarization-for-monitoring/?ref=blog.langchain.com#:~:text=We%20addressed%20these%20issues%20by,of%20our%20computer%20use%20capability)摘要。

![Image 8](https://blog.langchain.com/content/images/2025/07/image-7.png)

可以应用摘要的几个地方

在智能体设计的特定点[添加摘要](https://github.com/langchain-ai/open_deep_research/blob/e5a5160a398a3699857d00d8569cb7fd0ac48a4f/src/open_deep_research/utils.py?ref=blog.langchain.com#L1407)也可能很有用。例如，它可以用于后处理某些工具调用（例如，token密集的搜索工具）。作为第二个例子，[Cognition](https://cognition.ai/blog/dont-build-multi-agents?ref=blog.langchain.com#a-theory-of-building-long-running-agents)提到在智能体-智能体边界处进行摘要，以减少知识交接期间的token。如果需要捕获特定事件或决策，摘要可能是一个挑战。[Cognition](https://cognition.ai/blog/dont-build-multi-agents?ref=blog.langchain.com#a-theory-of-building-long-running-agents)为此使用了一个fine-tuned模型，这强调了这一步可能需要多少工作。

**Context修剪**

而摘要通常使用LLM来提取context的最相关部分，修剪通常可以过滤或，正如Drew Breunig指出的，"[修剪](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html?ref=blog.langchain.com)"context。这可以使用硬编码启发式，如从列表中删除[较旧的消息](https://python.langchain.com/docs/how_to/trim_messages/?ref=blog.langchain.com)。Drew还提到了[Provence](https://arxiv.org/abs/2501.16214?ref=blog.langchain.com)，一个用于问答的训练过的context修剪器。

### 隔离Context

_隔离context涉及将其分割以帮助智能体执行任务。_

**多智能体**

隔离context最流行的方法之一是将其分割到子智能体中。OpenAI [Swarm](https://github.com/openai/swarm?ref=blog.langchain.com)库的动机是[关注点分离](https://openai.github.io/openai-agents-python/ref/agent/?ref=blog.langchain.com)，其中智能体团队可以处理特定的子任务。每个智能体都有特定的工具集、指令和自己的context窗口。

![Image 9](https://blog.langchain.com/content/images/2025/07/image-8.png)

在多个智能体之间分割context

Anthropic的[多智能体研究员](https://www.anthropic.com/engineering/built-multi-agent-research-system?ref=blog.langchain.com)为此提出了理由：具有隔离context的多个智能体优于单智能体，主要是因为每个子智能体context窗口可以分配给更窄的子任务。正如博客所说：

> _[子智能体]并行操作，拥有自己的context窗口，同时探索问题的不同方面。_

当然，多智能体的挑战包括token使用（例如，据Anthropic报告，比聊天多达[15倍的token](https://www.anthropic.com/engineering/built-multi-agent-research-system?ref=blog.langchain.com)），需要仔细的[prompt工程](https://www.anthropic.com/engineering/built-multi-agent-research-system?ref=blog.langchain.com)来规划子智能体工作，以及子智能体的协调。

**使用环境进行Context隔离**

HuggingFace的[深度研究员](https://huggingface.co/blog/open-deep-research?ref=blog.langchain.com#:~:text=From%20building%20,it%20can%20still%20use%20it)展示了context隔离的另一个有趣例子。大多数智能体使用[工具调用API](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview?ref=blog.langchain.com)，它返回可以传递给工具（例如，搜索API）以获取工具反馈（例如，搜索结果）的JSON对象（工具参数）。HuggingFace使用[CodeAgent](https://huggingface.co/papers/2402.01030?ref=blog.langchain.com)，它输出包含所需工具调用的代码。然后代码在[沙箱](https://e2b.dev/?ref=blog.langchain.com)中运行。来自工具调用的选定context（例如，返回值）然后传递回LLM。

![Image 10](https://blog.langchain.com/content/images/2025/07/image-9.png)

沙箱可以将context从LLM中隔离出来。

这允许context在环境中与LLM隔离。Hugging Face注意到这是隔离token密集对象的绝佳方式：

> _[代码智能体允许]更好地处理状态...需要存储这个图像/音频/其他以供以后使用？没问题，只需将其分配为状态中的变量，你[以后可以使用它]。_

**状态**

值得指出的是，智能体的运行时[状态对象](https://langchain-ai.github.io/langgraph/concepts/low_level/?ref=blog.langchain.com#state)也可以是隔离context的绝佳方式。这可以与沙箱化起到相同的作用。状态对象可以设计为具有[模式](https://langchain-ai.github.io/langgraph/concepts/low_level/?ref=blog.langchain.com#schema)，该模式具有可以写入context的字段。模式的一个字段（例如，`messages`）可以在智能体的每一轮中暴露给LLM，但模式可以将信息隔离在其他字段中以供更有选择性的使用。

### 使用LangSmith / LangGraph进行Context工程

那么，你如何应用这些想法呢？在开始之前，有两个基础部分是有帮助的。首先，确保你有一种方法来[查看你的数据](https://hamel.dev/blog/posts/evals/?ref=blog.langchain.com)并跟踪智能体的token使用情况。这有助于告知在哪里最好地应用context工程努力。[LangSmith](https://docs.smith.langchain.com/?ref=blog.langchain.com)非常适合智能体[跟踪/可观察性](https://docs.smith.langchain.com/observability?ref=blog.langchain.com)，并提供了一个很好的方法来做到这一点。其次，确保你有一个简单的方法来测试context工程是否损害或改善智能体性能。LangSmith启用[智能体评估](https://docs.smith.langchain.com/evaluation/tutorials/agents?ref=blog.langchain.com)来测试任何context工程努力的影响。

**写入context**

LangGraph设计时考虑了线程范围的（[短期](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#short-term-memory)）和[长期记忆](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#long-term-memory)。短期记忆使用[检查点](https://langchain-ai.github.io/langgraph/concepts/persistence/?ref=blog.langchain.com)来在智能体的所有步骤中持久化[智能体状态](https://langchain-ai.github.io/langgraph/concepts/low_level/?ref=blog.langchain.com#state)。这作为"scratchpad"非常有用，允许你将信息写入状态并在智能体轨迹的任何步骤中获取它。

LangGraph的长期记忆让你能够跨智能体的_多个会话_持久化context。它很灵活，允许你保存小的[文件](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#profile)集（例如，用户配置文件或规则）或更大的记忆[集合](https://langchain-ai.github.io/langgraph/concepts/memory/?ref=blog.langchain.com#collection)。此外，[LangMem](https://langchain-ai.github.io/langmem/?ref=blog.langchain.com)提供了一套广泛的有用抽象来帮助LangGraph记忆管理。

**选择context**

在LangGraph智能体的每个节点（步骤）内，你可以获取[状态](https://langchain-ai.github.io/langgraph/concepts/low_level/?ref=blog.langchain.com#state)。这为你在每个智能体步骤中向LLM呈现什么context提供了细粒度的控制。

此外，LangGraph的长期记忆在每个节点内都是可访问的，并支持各种类型的检索（例如，获取文件以及[对记忆集合进行基于embedding的检索](https://langchain-ai.github.io/langgraph/cloud/reference/cli/?ref=blog.langchain.com#adding-semantic-search-to-the-store)）。有关长期记忆的概述，请参阅[我们的Deeplearning.ai课程](https://www.deeplearning.ai/short-courses/long-term-agentic-memory-with-langgraph/?ref=blog.langchain.com)。有关应用于特定智能体的记忆入门点，请参阅我们的[Ambient Agents](https://academy.langchain.com/courses/ambient-agents?ref=blog.langchain.com)课程。这展示了如何在可以管理你的电子邮件并从你的反馈中学习的长期运行智能体中使用LangGraph记忆。

![Image 11](https://blog.langchain.com/content/images/2025/07/image-10.png)

具有用户反馈和长期记忆的电子邮件智能体

对于工具选择，[LangGraph Bigtool](https://github.com/langchain-ai/langgraph-bigtool?ref=blog.langchain.com)库是对工具描述应用语义搜索的绝佳方式。这有助于在处理大量工具集合时为任务选择最相关的工具。最后，我们有几个[教程和视频](https://langchain-ai.github.io/langgraph/tutorials/rag/langgraph_agentic_rag/?ref=blog.langchain.com)展示如何在LangGraph中使用各种类型的RAG。

**压缩context**

因为LangGraph[是一个低级编排框架](https://blog.langchain.com/how-to-think-about-agent-frameworks/)，你[将智能体布局为一组节点](https://www.youtube.com/watch?v=aHCDrAbH_go&ref=blog.langchain.com)，[定义](https://blog.langchain.com/how-to-think-about-agent-frameworks/)每个节点内的逻辑，并定义在它们之间传递的状态对象。这种控制提供了几种压缩context的方法。

一种常见方法是使用消息列表作为智能体状态，并使用[一些内置实用程序](https://langchain-ai.github.io/langgraph/how-tos/memory/add-memory/?ref=blog.langchain.com#manage-short-term-memory)定期[摘要或修剪](https://langchain-ai.github.io/langgraph/how-tos/memory/add-memory/?ref=blog.langchain.com#manage-short-term-memory)它。但是，你也可以添加逻辑来后处理[工具调用](https://github.com/langchain-ai/open_deep_research/blob/e5a5160a398a3699857d00d8569cb7fd0ac48a4f/src/open_deep_research/utils.py?ref=blog.langchain.com#L1407)或智能体的工作阶段，有几种不同的方法。你可以在特定点添加摘要节点，或者也可以向工具调用节点添加摘要逻辑，以压缩特定工具调用的输出。

**隔离context**

LangGraph围绕[状态](https://langchain-ai.github.io/langgraph/concepts/low_level/?ref=blog.langchain.com#state)对象设计，允许你指定状态模式并在每个智能体步骤中访问状态。例如，你可以将工具调用的context存储在状态的某些字段中，将它们与LLM隔离，直到需要该context。除了状态之外，LangGraph还支持使用沙箱进行context隔离。请参阅此[repo](https://github.com/jacoblee93/mini-chat-langchain?tab=readme-ov-file&ref=blog.langchain.com)以获取使用[E2B沙箱](https://e2b.dev/?ref=blog.langchain.com)进行工具调用的LangGraph智能体示例。请参阅此[视频](https://www.youtube.com/watch?v=FBnER2sxt0w&ref=blog.langchain.com)以获取使用Pyodide进行沙箱化的示例，其中状态可以持久化。LangGraph还对构建多智能体架构有很多支持，如[supervisor](https://github.com/langchain-ai/langgraph-supervisor-py?ref=blog.langchain.com)和[swarm](https://github.com/langchain-ai/langgraph-swarm-py?ref=blog.langchain.com)库。你可以[查看](https://www.youtube.com/watch?v=4nZl32FwU-o&ref=blog.langchain.com)[这些](https://www.youtube.com/watch?v=JeyDrn1dSUQ&ref=blog.langchain.com)[视频](https://www.youtube.com/watch?v=B_0TNuYi56w&ref=blog.langchain.com)以获取有关在LangGraph中使用多智能体的更多详细信息。

### 结论

Context工程正在成为智能体构建者应该努力掌握的一门技艺。在这里，我们涵盖了今天许多流行智能体中看到的几种常见模式：

* _写入context - 将其保存在context窗口之外以帮助智能体执行任务。_
* _选择context - 将其拉入context窗口以帮助智能体执行任务。_
* _压缩context - 仅保留执行任务所需的token。_
* _隔离context - 将其分割以帮助智能体执行任务。_

LangGraph使实现每种方法变得容易，LangSmith提供了测试智能体和跟踪context使用的简单方法。LangGraph和LangSmith一起为识别应用context工程的最佳机会、实现它、测试它并重复这一过程提供了良性反馈循环。