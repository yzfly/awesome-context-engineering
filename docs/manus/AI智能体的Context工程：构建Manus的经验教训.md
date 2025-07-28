# AI智能体的Context工程：构建Manus的经验教训

原文链接：https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus

2025/7/18 --Yichao 'Peak' Ji

在[Manus](https://manus.im/app)项目的最初阶段，我和我的团队面临一个关键决策：我们应该使用开源基础模型训练一个端到端的智能体模型，还是基于前沿模型的[in-context learning](https://arxiv.org/abs/2301.00234)能力构建智能体？

回顾我在NLP领域的第一个十年，我们没有这样的选择余地。在[BERT](https://arxiv.org/abs/1810.04805)的遥远时代（是的，已经过去七年了），模型必须经过fine-tuning和评估才能迁移到新任务。即使模型相比今天的LLM来说非常小，这个过程每次迭代通常需要数周时间。对于快速发展的应用，特别是在产品市场契合度（PMF）之前，如此缓慢的反馈循环是致命的。这是我上一个创业公司的痛苦教训，当时我从零开始训练模型用于[开放信息抽取](https://en.wikipedia.org/wiki/Open_information_extraction)和语义搜索。然后[GPT-3](https://arxiv.org/abs/2005.14165)和[Flan-T5](https://arxiv.org/abs/2210.11416)出现了，我的内部模型一夜之间变得无关紧要。讽刺的是，正是这些模型标志着in-context learning的开始——以及一条全新的前进道路。

这个来之不易的教训让选择变得明确：Manus将押注于context工程。这使我们能够在几小时而不是几周内发布改进，并保持我们的产品与底层模型正交：如果模型进步是涨潮，我们希望Manus是船，而不是固定在海床上的柱子。

然而，context工程绝非简单直接。这是一门实验科学——我们已经重建了四次智能体框架，每次都是在发现更好的context塑造方法之后。我们亲切地将这种手动的架构搜索、prompt调整和经验猜测过程称为"随机梯度下降"（Stochastic Graduate Descent）。虽然不够优雅，但确实有效。

这篇文章分享了我们通过自己的"SGD"达到的局部最优解。如果你正在构建自己的AI智能体，我希望这些原则能帮助你更快地收敛。

### 围绕KV-Cache进行设计

如果我必须选择一个指标，我认为KV-cache命中率是生产阶段AI智能体最重要的单一指标。它直接影响延迟和成本。要理解原因，让我们看看[典型智能体](https://arxiv.org/abs/2210.03629)是如何运作的：

在接收到用户输入后，智能体通过一系列工具使用来完成任务。在每次迭代中，模型基于当前context从预定义的动作空间中选择一个动作。然后在环境中执行该动作（例如，Manus的虚拟机沙箱）以产生观察结果。动作和观察结果被附加到context中，形成下一次迭代的输入。这个循环持续进行，直到任务完成。

可以想象，context随着每一步而增长，而输出——通常是结构化的函数调用——保持相对较短。这使得智能体中的预填充与解码比率相比聊天机器人高度倾斜。例如，在Manus中，平均输入与输出token比率约为100:1。

幸运的是，具有相同前缀的context可以利用[KV-cache](https://medium.com/@joaolages/kv-caching-explained-276520203249)，这大大减少了首token时间（TTFT）和推理成本——无论你使用的是自托管模型还是调用推理API。我们说的不是小幅节省：例如，使用Claude Sonnet时，缓存的输入token成本为0.30美元/MTok，而未缓存的成本为3美元/MTok——相差10倍。

![Image 1](https://d1oupeiobkpcny.cloudfront.net/user_upload_by_module/markdown/310708716691272617/OhdKxGRSXCcuqOvz.png)

从context工程的角度来看，提高KV-cache命中率涉及几个关键实践：

1. **保持prompt前缀稳定**。由于LLM的[自回归](https://en.wikipedia.org/wiki/Autoregressive_model)特性，即使是单个token的差异也可能使该token之后的缓存失效。一个常见错误是在系统prompt开头包含时间戳——特别是精确到秒的时间戳。当然，这让模型能够告诉你当前时间，但也会破坏你的缓存命中率。

2. **使你的context仅追加**。避免修改之前的动作或观察结果。确保你的序列化是确定性的。许多编程语言和库在序列化JSON对象时不保证稳定的键排序，这可能会悄无声息地破坏缓存。

3. **在需要时明确标记缓存断点**。一些模型提供商或推理框架不支持自动增量前缀缓存，而是需要在context中手动插入缓存断点。分配这些断点时，要考虑潜在的缓存过期，至少确保断点包含系统prompt的结尾。

此外，如果你使用[vLLM](https://github.com/vllm-project/vllm)等框架自托管模型，请确保启用[前缀/prompt缓存](https://docs.vllm.ai/en/stable/design/v1/prefix_caching.html)，并使用会话ID等技术在分布式worker之间一致地路由请求。

### 掩码，而非移除

随着你的智能体承担更多能力，其动作空间自然变得更加复杂——简单来说，工具数量激增。最近[MCP](https://modelcontextprotocol.io/introduction)的流行只是火上浇油。如果你允许用户可配置的工具，相信我：总会有人将数百个神秘工具插入你精心策划的动作空间。结果，模型更可能选择错误的动作或采取低效的路径。简而言之，你的重装智能体变得更愚蠢了。

自然的反应是设计动态动作空间——也许使用类似[RAG](https://en.wikipedia.org/wiki/Retrieval-augmented_generation)的方法按需加载工具。我们在Manus中也尝试过这种方法。但我们的实验表明了一个明确的规则：除非绝对必要，避免在迭代中动态添加或移除工具。主要有两个原因：

1. 在大多数LLM中，工具定义在序列化后位于context的前部，通常在系统prompt之前或之后。因此任何更改都会使所有后续动作和观察的KV-cache失效。

2. 当之前的动作和观察仍然引用当前context中不再定义的工具时，模型会感到困惑。没有[约束解码](https://platform.openai.com/docs/guides/structured-outputs)，这通常导致模式违规或幻觉动作。

为了解决这个问题同时仍然改进动作选择，Manus使用上下文感知的[状态机](https://en.wikipedia.org/wiki/Finite-state-machine)来管理工具可用性。它不是移除工具，而是在解码期间掩码token logits，以根据当前context防止（或强制）选择某些动作。

![Image 2](https://d1oupeiobkpcny.cloudfront.net/user_upload_by_module/markdown/310708716691272617/cWxINCvUfrmlbvfV.png)

在实践中，大多数模型提供商和推理框架都支持某种形式的响应预填充，这允许你在不修改工具定义的情况下约束动作空间。通常有三种函数调用模式（我们将使用NousResearch的[Hermes格式](https://github.com/NousResearch/Hermes-Function-Calling)作为示例）：

• **Auto** – 模型可以选择调用函数或不调用。通过仅预填充回复前缀实现：`<|im_start|>assistant`

• **Required** – 模型必须调用函数，但选择不受约束。通过预填充到工具调用token实现：`<|im_start|>assistant<tool_call>`

• **Specified** – 模型必须从特定子集调用函数。通过预填充到函数名开头实现：`<|im_start|>assistant<tool_call>{"name": "browser_`

使用这种方法，我们通过直接掩码token logits来约束动作选择。例如，当用户提供新输入时，Manus必须立即回复而不是采取动作。我们还故意设计了具有一致前缀的动作名称——例如，所有浏览器相关工具都以`browser_`开头，命令行工具以`shell_`开头。这使我们能够轻松强制智能体在给定状态下仅从某组工具中选择，而无需使用有状态的logits处理器。

这些设计有助于确保Manus智能体循环保持稳定——即使在模型驱动的架构下。

### 将文件系统用作Context

现代前沿LLM现在提供128K token或更多的context窗口。但在现实世界的智能体场景中，这通常是不够的，有时甚至是负担。有三个常见的痛点：

1. **观察结果可能很庞大**，特别是当智能体与网页或PDF等非结构化数据交互时。很容易超过context限制。

2. **模型性能往往在超过某个context长度后下降**，即使窗口在技术上支持它。

3. **长输入很昂贵**，即使有前缀缓存。你仍然需要为传输和预填充每个token付费。

为了处理这个问题，许多智能体系统实现context截断或压缩策略。但过于激进的压缩不可避免地导致信息丢失。问题是根本性的：智能体本质上必须基于所有先前状态预测下一个动作——而你无法可靠地预测哪个观察结果可能在十步后变得关键。从逻辑角度来看，任何不可逆的压缩都带有风险。

这就是为什么我们将文件系统视为Manus中的终极context：大小无限、本质上持久，并且可以由智能体本身直接操作。模型学会按需写入和读取文件——将文件系统不仅用作存储，而且用作结构化的外部化内存。

![Image 3](https://d1oupeiobkpcny.cloudfront.net/user_upload_by_module/markdown/310708716691272617/sBITCOxGnHNUPHTD.png)

我们的压缩策略总是设计为可恢复的。例如，只要保留URL，网页内容就可以从context中删除；只要路径在沙箱中仍然可用，文档内容就可以省略。这使Manus能够缩短context长度而不会永久丢失信息。

在开发这个功能时，我发现自己在想象状态空间模型（SSM）在智能体设置中有效工作需要什么。与Transformer不同，SSM缺乏完全注意力机制，在长程后向依赖方面存在困难。但如果它们能够掌握基于文件的内存——外部化长期状态而不是在context中保持——那么它们的速度和效率可能会解锁新一类智能体。智能体SSM可能是[神经图灵机](https://arxiv.org/abs/1410.5401)的真正继承者。

### 通过复述操纵注意力

如果你使用过Manus，你可能注意到了一些有趣的事情：在处理复杂任务时，它倾向于创建一个todo.md文件——并在任务进行时逐步更新它，勾选已完成的项目。

这不仅仅是可爱的行为——这是一个操纵注意力的深思熟虑的机制。

![Image 4](https://d1oupeiobkpcny.cloudfront.net/user_upload_by_module/markdown/310708716691272617/OYpTzfPZaBeeWFOx.png)

Manus中的典型任务平均需要大约50次工具调用。这是一个长循环——由于Manus依赖LLM进行决策，它容易偏离主题或忘记早期目标，特别是在长context或复杂任务中。

通过不断重写待办事项列表，Manus将其目标复述到context的末尾。这将全局计划推入模型的最近注意力范围，避免"迷失在中间"的问题并减少目标错位。实际上，它使用自然语言来偏向自己对任务目标的关注——而无需特殊的架构更改。

### 保留错误内容

智能体会犯错误。这不是bug——这是现实。语言模型会产生幻觉，环境返回错误，外部工具行为异常，意外的边缘情况总是出现。在多步任务中，失败不是例外；它是循环的一部分。

然而，一个常见的冲动是隐藏这些错误：清理跟踪、重试动作，或重置模型状态并依赖神奇的"[temperature](https://arxiv.org/abs/2405.00492)"。这感觉更安全、更可控。但这是有代价的：擦除失败会移除证据。没有证据，模型无法适应。

![Image 5](https://d1oupeiobkpcny.cloudfront.net/user_upload_by_module/markdown/310708716691272617/dBjZlVbKJVhjgQuF.png)

根据我们的经验，改善智能体行为最有效的方法之一看似简单：将错误转向留在context中。当模型看到失败的动作——以及由此产生的观察或堆栈跟踪——它隐式地更新其内部信念。这将其先验从类似动作中转移，减少重复相同错误的机会。实际上，我们认为错误恢复是真正智能体行为最清晰的指标之一。然而，它在大多数学术工作和公共基准测试中仍然代表不足，这些通常关注理想条件下的任务成功。

### 不要被Few-Shot困住

[Few-shot prompting](https://www.promptingguide.ai/techniques/fewshot)是改善LLM输出的常见技术。但在智能体系统中，它可能以微妙的方式适得其反。

语言模型是优秀的模仿者；它们模仿context中的行为模式。如果你的context充满了类似的过去动作-观察对，模型会倾向于遵循该模式，即使它不再是最优的。

这在涉及重复决策或动作的任务中可能很危险。例如，当使用Manus帮助审查20份简历时，智能体经常陷入节奏——重复类似的动作，仅仅因为这是它在context中看到的。这导致漂移、过度泛化，或有时产生幻觉。

![Image 6](https://d1oupeiobkpcny.cloudfront.net/user_upload_by_module/markdown/310708716691272617/IIyBBdwwuMDJUnUc.png)

解决方案是增加多样性。Manus在动作和观察中引入少量结构化变化——不同的序列化模板、替代措辞、顺序或格式的轻微噪声。这种受控的随机性有助于打破模式并调整模型的注意力。换句话说，不要让自己陷入few-shot的困境。你的context越统一，你的智能体就越脆弱。

### 结论

Context工程仍然是一门新兴科学——但对于智能体系统来说，它已经是必不可少的。模型可能变得更强、更快、更便宜，但再多的原始能力也无法替代对内存、环境和反馈的需求。你如何塑造context最终定义了你的智能体如何行为：运行多快、恢复多好、扩展多远。

在Manus，我们通过反复重写、死胡同和数百万用户的真实世界测试学到了这些教训。我们在这里分享的内容都不是普遍真理——但这些是对我们有效的模式。如果它们能帮助你避免哪怕一次痛苦的迭代，那么这篇文章就完成了它的使命。

智能体的未来将一次一个context地构建。好好工程化它们。