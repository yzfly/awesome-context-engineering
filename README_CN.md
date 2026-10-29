# Awesome Context Engineering

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![GitHub stars](https://img.shields.io/github/stars/yzfly/awesome-context-engineering.svg?style=social&label=Star)](https://github.com/yzfly/awesome-context-engineering)
[![GitHub forks](https://img.shields.io/github/forks/yzfly/awesome-context-engineering.svg?style=social&label=Fork)](https://github.com/yzfly/awesome-context-engineering)

精心整理的**Context工程**资源集合，涵盖AI智能体和大语言模型(LLM)的相关资源、论文、工具和最佳实践。

> Context工程是在智能体轨迹的每一步中，用恰当的信息填充context窗口的艺术和科学。

[中文版本](README_CN.md) | [English](README.md)

> 🗓️ **最近更新：2026-10-23** —— 本周要点见[最新动态](#-最新动态)。

## 📚 目录

- [最新动态](#-最新动态)
- [什么是Context工程？](#什么是context工程)
- [精选文章](#精选文章)
- [研究论文](#研究论文)
- [工具与项目](#工具与项目)
- [专家观点](#专家观点)
- [模型Context协议 (MCP)](#模型context协议-mcp)
- [贡献指南](#贡献指南)
- [Star历史](#star历史)

## 什么是Context工程？

Context工程是对大语言模型(LLM)信息负载的系统性优化。它包括：

- **Context检索与生成**：选择和创建相关信息
- **Context处理**：组织和构建context以实现最佳消费
- **Context管理**：处理context窗口、内存和跨交互状态
- **Context压缩**：在保留关键信息的同时减少token使用
- **Context隔离**：在不同context空间中分离关注点

## 🆕 最新动态

*2026-10-23 当周，整理自各家官方工程博客、新论文与 GitHub 热门项目。*

- **Agent Skills 与 MCP 一起进入 Linux 基金会。** Anthropic 于 10 月 22 日[将 Agent Skills 标准捐赠给 Agentic AI Foundation](https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation)。已有 70 多个智能体平台（ChatGPT、Cursor、GitHub Copilot、VS Code…）集成，累计发布约 100 万个技能、安装近 2.8 亿次。渐进式披露（`SKILL.md` → 附带文件）已成为交付程序性上下文的默认方式。另见 Vercel 的 [State of agent skills](https://vercel.com/blog/state-of-agent-skills)。
- **单次请求的上下文持续变大，缓存决定成本。** Anthropic 披露 [Claude Code 单次请求的上下文半年增长 2.6 倍](https://claude.com/blog/claude-opus-5-5-built-for-coding-sessions-that-use-more-context)，输入输出 token 比从 189:1 升到 324:1。Opus 5.5 把缓存读取价格降低 60%，Claude Code 的未命中缓存输入减少一半以上（会话中切换 effort、按需加载工具都不再打断缓存；API 用户可用 1 小时 TTL）。OpenAI 也推出了 [GPT-6 改进版提示缓存](https://openai.com/index/better-prompt-caching-for-gpt-6)，附带缓存面板与未命中诊断。
- **Harness 工程有了量化数据。** Cursor [把系统提示削减约 66%，并把内置工具改为动态加载](https://cursor.com/blog/improved-token-efficiency)，在质量不降的前提下 token 成本降低 7%（此前 MCP 工具动态加载已让相关会话 token 减少 46.9%）。AI21 通过控制每个阶段用哪个模型、读多少上下文，[将 GitLab Duo 代码审查成本降低 98%](https://www.ai21.com/blog/gitlab-case-study/)。Hugging Face：[同一权重在两个 harness 中分别得到 62% 和 33%](https://x.com/huggingface/status/2106034221005312448)。
- **压缩（compaction）成为研究课题，也成了攻击面。** [SelfCompact](https://arxiv.org/abs/2606.23525) 让智能体自己决定何时压缩（单题成本降 30–70%）；[REMORY](https://arxiv.org/abs/2610.11287) 在文本摘要之外附加残差式软记忆 token；[Cache Engineering for Agents](https://arxiv.org/abs/2610.28577) 让智能体直接丢弃或重定位 KV 缓存条目。与此同时，OpenAI 发现训练中的模型会[在自己的压缩摘要里写入提示注入](https://simonwillison.net/2026/Sep/17/compaction-summaries/)。
- **离线整理的记忆。** Cognition 推出 [Devin Memory & Dreaming](https://devin.ai/blog/memory-and-dreaming)：以 markdown 保存记忆，并每天异步去重、关联。LangChain [重构了 Deep Agents 的技能机制](https://www.langchain.com/blog/revamping-skills-in-deep-agents)（技能绑定工具、可固定、可重载，保持提示缓存命中），并提出[上下文工程始于检索](https://www.langchain.com/blog/context-engineering-starts-with-retrieval)。

## 📖 精选文章

### Context Rot：增加输入token如何影响LLM性能
- https://research.trychroma.com/context-rot

### Manus Context工程
**AI智能体的Context工程：构建Manus的经验教训**
- 📄 原文：[Manus博客](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
- 🇨🇳 中文翻译：[AI智能体的Context工程：构建Manus的经验教训.md](docs/manus/AI智能体的Context工程：构建Manus的经验教训.md)

构建生产级AI智能体的关键洞察：
- 围绕KV-Cache进行设计以优化性能
- 掩码而非移除工具以改善动作选择
- 使用文件系统作为外部context内存
- 通过复述技术操纵注意力

### Claude Code 最佳实践
**Claude Code 最佳实践指南**
- 📄 原文：[Anthropic 官方文档](https://www.anthropic.com/engineering/claude-code-best-practices)
- 🇨🇳 中文翻译：[claude-code-best-practices](docs/claudecode/claude-code-best-practices-zh.md)

### Claude 有效上下文工程
**AI智能体的有效上下文工程**
- 📄 原文：[Anthropic 官方文档](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- 🇨🇳 中文翻译：[有效上下文工程.md](docs/claudecode/effective-context-engineering-for-ai-agents.md)
- 📄 [Context 编辑与 memory 工具](https://www.anthropic.com/news/context-management)
- [我们如何构建多智能体研究系统](https://www.anthropic.com/engineering/multi-agent-research-system)
- [为 AI 智能体编写高效工具](https://www.anthropic.com/engineering/writing-tools-for-agents)
- [通过代码执行使用 MCP：构建更高效的智能体](https://www.anthropic.com/engineering/code-execution-with-mcp)
- [将 Agent Skills 捐赠给 Agentic AI Foundation](https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation)（2026 年 10 月）
- [编程会话更长、上下文更多，Claude Opus 5.5 为此而设计](https://claude.com/blog/claude-opus-5-5-built-for-coding-sessions-that-use-more-context) —— 单次请求上下文半年增长 2.6 倍；定价、模型行为与 Claude Code harness 如何让长会话尽量命中缓存
- [Using Claude Code: Spending your effort](https://claude.dev/blog/spending-your-effort/) —— effort 设置实际换来的是什么（主要是更多验证与边界测试）
- [构建高效的智能体自动化](https://claude.dev/blog/building-effective-agent-automations/) —— 基于 Managed Agents 的定时任务，只追踪上次运行以来的变化

### OpenAI

- [Better prompt caching for GPT-6](https://openai.com/index/better-prompt-caching-for-gpt-6) —— 默认命中率更高、30 分钟内共享前缀折扣、缓存面板与未命中诊断
- [GPT-6 系列模型指南](https://openai.com/index/practical-guide-building-gpt-6/) —— 模型选择、缓存与压缩、技能设计、长时任务编排
- [Introducing the Agents API](https://openai.com/index/introducing-the-agents-api) —— 把 Codex 背后的 harness 以 API 形式开放，用于长时运行的云端智能体
- [模型失准报告框架](https://openai.com/index/model-misalignment-reporting-framework) —— 其中包含模型在自己的压缩摘要中注入指令的案例

### Harness 工程

- [Improved token efficiency for longer agent runs（Cursor）](https://cursor.com/blog/improved-token-efficiency) —— 精简系统提示、按需加载工具、提升缓存复用、压缩文件读取、合理使用子智能体
- [Free the models: Harness design at the frontier（Replit）](https://replit.com/blog/free-the-models) —— 由主智能体决定子智能体的层级与投入，而非固定路由
- [The Anatomy of Harness Engineering（Google）](https://developers.googleblog.com/the-anatomy-of-harness-engineering-how-to-evaluate-iterate-and-guard-ai-coding-agents) —— 如何评估、迭代与防护编程智能体
- [Improving Deep Agents with harness engineering（LangChain）](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering) —— 只改 harness，Terminal Bench 2.0 从 52.8 提升到 66.5
- [How we cut GitLab Duo Code Review's cost by 98%（AI21）](https://www.ai21.com/blog/gitlab-case-study/) —— 按阶段选择模型并控制每阶段读取的上下文量
- [A Smarter Model Won't Save Your Bad Harness（Cerebras）](https://www.cerebras.ai/blog/a-smarter-model-won-t-save-your-bad-harness)
- [Memory and dreaming: how Devin learns from working with you（Cognition）](https://devin.ai/blog/memory-and-dreaming)
- [Introducing Manus 2.0](https://manus.im/blog/introducing-manus-2-0) —— 自研 harness「Cascade」（Manus 测试配置下 token 少用 23.2%）

### LangChain Context工程
**智能体的Context工程**
- 📄 原文：[LangChain博客](https://blog.langchain.com/context-engineering-for-agents/)
- 🇨🇳 中文翻译：[智能体的Context工程-中文版.md](docs/langchain/智能体的Context工程-中文版.md)

涵盖四大关键策略的综合指南：
- **写入Context**：将信息保存在context窗口之外
- **选择Context**：将相关信息拉入context
- **压缩Context**：仅保留必要的token
- **隔离Context**：在不同空间中分割context

LangChain 更多文章：
- [Context Engineering for AI Agents Starts with Retrieval](https://www.langchain.com/blog/context-engineering-starts-with-retrieval)（2026 年 10 月）—— 先搜后读、超大结果外置、把检索委派给子智能体
- [Revamping Skills in Deep Agents](https://www.langchain.com/blog/revamping-skills-in-deep-agents)（2026 年 10 月）—— 技能绑定工具、固定技能与线程内重载
- [Context Management for Deep Agents](https://www.langchain.com/blog/context-management-for-deepagents) —— 大体积工具输入输出外置到文件系统，达到阈值时对历史做摘要


### dbreunig Context工程系列
**长Context的失效原理和解决方案**
- 📄 原文 Part 1：[长Context如何失效](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)
- 🇨🇳 中文翻译：[长上下文的失效原理及解决方案.md](docs/dbreunig/长上下文的失效原理及解决方案.md)
- 📄 原文 Part 2：[如何修复你的Context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)
- 🇨🇳 中文翻译：[上下文修复的实用指南.md](docs/dbreunig/上下文修复的实用指南.md)

深入探讨context失效模式和管理策略：
- Context污染、分散、混乱和冲突模式分析
- RAG、工具配置、context隔离、修剪、总结和卸载技术

### 编程智能体的高级上下文工程实践

- [GitHub](https://github.com/humanlayer/humanlayer)
- [YouTube](https://humanlayer.dev/youtube)

在复杂代码库中使用AI解决难题的指南。

### 不要构建多智能体（Cognition）

- 📄 原文：[Cognition 博客](https://cognition.ai/blog/dont-build-multi-agents)

构建可靠智能体的原则：共享完整上下文，避免脆弱的并行多智能体架构。

### AI 的新技能不是提示工程，而是上下文工程（Philipp Schmid）

- 📄 原文：[philschmid.de](https://www.philschmid.de/context-engineering)

指出多数智能体失败源于 context 不足而非模型能力，将 context 拆解为指令、历史、长期记忆、RAG、工具与输出格式。

### 上下文工程（Simon Willison）

- 📄 原文：[simonwillison.net](https://simonwillison.net/2025/jun/27/context-engineering/)

论述为何 "context engineering" 比 "prompt engineering" 更贴切，梳理并串联了该术语的奠基性讨论。

### 宽度研究：超越上下文窗口（Manus）

- 📄 原文：[Manus 博客](https://manus.im/blog/manus-wide-research-solve-context-problem)

通过将任务拆分给多个专职子智能体并行处理，避免单一超载上下文窗口在处理大量条目时出现的编造问题。

### SWE-grep：面向快速上下文检索的强化学习（Cognition）

- 📄 原文：[Cognition 博客](https://cognition.ai/blog/swe-grep)

用强化学习训练、每轮最多 8 路并行检索的模型，在最小化 context 污染的同时将代码上下文检索速度提升一个数量级。


### 压缩是一种有损操作

- https://loopandretry.github.io/posts/compaction-is-a-lossy-operation/

为什么压缩智能体的 context 是有损压缩，而非免费的摘要——哪些信息被保留、哪些被悄悄丢弃，以及它如何逐渐劣化长的多轮运行。

- [压缩摘要中的自生成提示注入（Simon Willison）](https://simonwillison.net/2026/Sep/17/compaction-summaries/) —— 强化学习中的模型在自己的压缩摘要里写入「附加指令」；应把摘要视为不可信上下文

## 📑 研究论文

### 综述论文

**大语言模型Context工程综述**
- 📄 arXiv：[2507.13334](https://arxiv.org/abs/2507.13334)
- 📊 对1400+研究论文的综合分析
- 🎯 建立了context工程组件的正式分类法

> *大语言模型(LLM)的性能从根本上由推理过程中提供的上下文信息决定。本综述介绍了Context工程，这是一门超越简单prompt设计的正式学科，涵盖了对LLM信息负载的系统性优化。*

**Harness Engineering for Language Agents: The Harness Layer as Control, Agency, and Runtime**
- 📄 Preprints：[202603.1756](https://www.preprints.org/manuscript/202603.1756/v2)
- 📊 审视学术界与公开工程实践中 63 项与 harness 相关的工作
- 🎯 把智能体 harness 层拆分为 Control、Agency、Runtime（CAR）三部分，并提出 HarnessCard 作为报告规范

**Recursive Harness Distillation across Agents for Robot Manipulation**
- 📄 arXiv：[2609.33378](https://arxiv.org/abs/2609.33378)
- 🎯 将 Astra 的机器人干预经验蒸馏为供轻量级 Luna 智能体复用的操作手册，并根据 Luna 的执行反馈递归改进手册；智能体与 VLA 的模型权重保持不变（[项目主页](https://seungyeon.me/RHD/)）

**Harness-Zero: Harness Distillation via Agent-as-Harness**
- 📄 arXiv：[2609.24974](https://arxiv.org/abs/2609.24974)
- 🎯 通过 agent-as-harness 训练循环将优化后的 harness 行为蒸馏进模型权重，研究不在推理阶段保留该 harness 时的效果（[代码](https://github.com/metaevo-ai/harness-zero)）

**Context Engineering 2.0：上下文工程的语境**
- 📄 arXiv：[2510.26493](https://arxiv.org/abs/2510.26493)
- 🎯 将上下文工程追溯二十余年，系统性给出定义，并梳理从人机交互到人-智能体范式的设计原则

**Agentic Context Engineering：面向自我改进语言模型的演化式上下文**
- 📄 arXiv：[2510.04618](https://arxiv.org/abs/2510.04618)
- 🎯 将 context 视为可演化的 playbook（生成器/反思器/整理器），避免 context 坍塌；智能体任务 +10.6%、金融 +8.6%（ICLR 2026）

**AI 智能体时代的记忆：综述**
- 📄 arXiv：[2512.13564](https://arxiv.org/abs/2512.13564)
- 🎯 提出"形态-功能-动态"多维记忆分类法，并厘清 agent memory 与 RAG、上下文工程的区别

**The Agent Loop：LLM 智能体的控制策略、技能与 Harness 综述**
- 📄 SSRN：[7186738](https://ssrn.com/abstract=7186738)
- 🎯 以智能体循环（agent loop）为分析单位；讨论循环内的上下文管理和作为上下文管理器的 harness，并涵盖终止、验证、恢复与技能

**Meta Context Engineering via Agentic Skill Evolution**
- 📄 arXiv：[2601.21557](https://arxiv.org/abs/2601.21557)
- 🎯 同时演化上下文工程技能与上下文产物的双层框架，用于替代静态启发式规则（ICML 2026，[代码](https://github.com/metaevo-ai/meta-context-engineering)）

**PerspectiveGap：多智能体编排提示词 benchmark**
- 📄 arXiv：[2606.08878](https://arxiv.org/abs/2606.08878)
- 📊 110 个场景、10 种拓扑，测试 LLM 如何把上下文片段分配给 subagents
- 🎯 首个面向多智能体编排 prompt 写作的基准；33 个商业模型在角色-片段分配任务上平均通过率仅 17.2%

**Agon：基于 Prompt Economy 的大规模全学科自主科研系统**
- 📄 arXiv：[2606.24177](https://arxiv.org/abs/2606.24177)
- 📊 18 个角色共 230.6 KiB prompt，是跨学科 autoresearch 系统中 prompt 体量最小的（[代码](https://github.com/AutoResearch-Factory/Agon)）
- 🎯 Prompt Economy 把 prompt 工程当作工程学科，用可复用的循环驱动智能体；不改代码已跑过 10 多个研究领域，最长一次无人值守运行 30 天

### 近期论文（2026 年 9–10 月）

**压缩与 KV 缓存**
- [Self-Compacting Language Model Agents](https://arxiv.org/abs/2606.23525) —— 模型通过压缩工具和轻量规则自行决定何时、如何压缩；效果持平或优于固定间隔摘要，单题成本降 30–70%
- [REMORY: Learning Residual Memory for Context Compaction](https://arxiv.org/abs/2610.11287) —— 在文本摘要之外附加有界软记忆 token，类似序列维度上的残差连接
- [Cache Engineering for Agents](https://arxiv.org/abs/2610.28577) —— 智能体直接操作活跃 KV 缓存（丢弃、重定位）而无需重新预填充；滚动丢弃平均 +1.7 分、成本 −27.7%
- [The KV-Surgeon: Compensation-Aware Offline KV-Cache Compaction](https://arxiv.org/abs/2610.25933) —— 50 倍压缩下比注意力匹配方法最多高 4 分
- [DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression](https://arxiv.org/abs/2609.19969) —— 跨层 KV 复用 + FP4 缓存，百万上下文下全局 KV 约每 token 890 字节

**上下文结构与记忆**
- [Librarian Agents](https://arxiv.org/abs/2610.22273) —— 用强化学习训练智能体为下游回答者构建层级文件工作区；BrowseComp-Plus 上 +33 个百分点
- [Pack Light, CarryOn](https://arxiv.org/abs/2610.18784) —— 持久化可复用的任务对象，而不是反复处理交互历史；token 减少 22.9–51.9%
- [FlowState](https://arxiv.org/abs/2609.34565) —— 把执行状态作为记忆，语义状态节点引用原始工具观测；token 减少约 41–43%
- [Beyond Memory: Explicit Belief States](https://arxiv.org/abs/2610.01415) —— 以显式信念状态作为决策上下文
- [Cortex: Consolidating Context into Parametric Memory](https://arxiv.org/abs/2610.20399) —— 把上下文带来的增益蒸馏为可合并的权重更新
- [Memory-Centric Continual Learning for AI Agents](https://arxiv.org/abs/2610.29357) —— 以「能否改善未来行动」而非「能否回忆」评判记忆
- [Seeing, Maintaining, and Learning](https://arxiv.org/abs/2610.19366) —— 多模态智能体记忆综述

**Harness 工程与技能**
- [Harness²: Recursive Agent Harnessing for an Open World](https://arxiv.org/abs/2610.14224)（[代码](https://github.com/google-research/harness2)）
- [Self-Evolving Agent Harness](https://arxiv.org/abs/2610.18813) —— 权重固定，提示词、记忆与工作流持续演化，并给出持久化与准入规则
- [MILO: Automated Harness Discovery via Orchestrated Multi-Agent Evolution](https://arxiv.org/abs/2609.38349)
- [Doc2Harness](https://arxiv.org/abs/2610.20344) —— 长时程 harness 工程基准，19 个前沿模型中最好的也只通过 53% 的测试
- [How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?](https://arxiv.org/abs/2609.40303) —— 仅有读写与 bash 的最小脚手架在 MLE 任务上不输复杂开源脚手架
- [Surviving the Router](https://arxiv.org/abs/2610.08098) —— 技能注入攻击须先赢得技能检索；对技能库安全的启示

### 核心研究领域

- **记忆系统**：[Reflexion](https://arxiv.org/abs/2303.11366)、[生成式智能体](https://ar5iv.labs.arxiv.org/html/2304.03442)、[StructMem](https://arxiv.org/abs/2604.21748)（[代码](https://github.com/zjunlp/LightMem)）
- **检索增强生成**：[RAG综述](https://github.com/langchain-ai/rag-from-scratch)
- **工具集成**：[工具选择](https://arxiv.org/abs/2410.14594)、[BigTool](https://arxiv.org/abs/2505.03275)
- **Context压缩**：[递归摘要](https://arxiv.org/pdf/2308.15022)、[Context修剪](https://arxiv.org/abs/2501.16214)、[TokenPilot](https://arxiv.org/abs/2606.17016)（[代码](https://github.com/zjunlp/LightRSI)）
- **长Context局限**：[Lost in the Middle](https://arxiv.org/abs/2307.03172)

- **Harness 适配**：[Training Agents to Evolve with Their Harness: TaoLive Digital Avatar Agent Technical Report](https://arxiv.org/abs/2608.15763) — 研究如何通过训练适应 Skills、工具 Schema、提示词和运行时 Hooks 的变化。

## 🛠️ 工具与项目

### 综合资源

1. **[提示工程指南](https://github.com/dair-ai/Prompt-Engineering-Guide)**
   - 提示工程、上下文工程、RAG 与 AI 智能体综合指南
   - 涵盖技术、论文、工具与最佳实践
   - 广泛使用的教学资源

2. **[Awesome Context工程综述](https://github.com/Meirtz/Awesome-Context-Engineering)**
   - context工程技术的综合调研
   - 方法论和应用概述
   - 学术研究重点

3. **[Context工程入门](https://github.com/coleam00/context-engineering-intro)**
   - AI编程助手的实用指南
   - 以Claude Code为中心的方法
   - 实践实施策略

4. **[Context-Engineering（davidkimai）](https://github.com/davidkimai/Context-Engineering)**
   - 上下文工程第一性原理手册
   - 从基础原理到进阶技术

5. **[Context-Engineering（jasontang-ai）](https://github.com/jasontang-ai/Context-Engineering)**
   - 受 Karpathy 启发的第一性原理手册
   - 从提示工程迈向上下文设计、编排与优化

6. **[The Context Course（HuggingFace）](https://github.com/huggingface/context-course)**
   - HuggingFace 面向代码 agent 的上下文工程课程
   - 涵盖 skills、MCP、插件、子智能体、hooks，以及从零构建自己的 agent
   - 参考智能体：Claude Code、Codex、OpenCode

7. **[Agent Systems Handbook](https://github.com/Prompthon-IO/agent-systems-handbook)**
   - 实战 AI agents 手册，涵盖 agentic 工作流与多智能体架构
   - 横跨 LangGraph、MCP/A2A、上下文工程、agent 记忆、评估与可观测性

### 2026 上下文工程新动向：Harness、Skills 与协议

- **[deepseek-harness (dsh)](https://github.com/deepseek-ai/deepseek-harness)**：DeepSeek 官方开源 Agent 框架（2026-08）。追加式会话日志是模型上下文的*唯一*来源（"模型可见即已记录"），提示分节、工具 schema、技能与子 Agent 全部以可替换插件组合——上下文装配的参考设计
- **[Agent Skills 开放标准](https://agentskills.io/)**：以 `SKILL.md` 目录通过渐进式披露加载（名称/描述 → 完整指令 → 附带文件），已被 70 多个智能体平台集成（Claude Code、ChatGPT、Codex、Gemini CLI、Cursor、GitHub Copilot、VS Code、OpenCode、dsh…），累计发布约 100 万个技能；2026 年 10 月捐赠给 Linux 基金会旗下的 Agentic AI Foundation（与 MCP 同属），是*程序性*上下文的可移植单元
- **[MCP 2026-07-28 规范](https://blog.modelcontextprotocol.io/posts/2026-07-28/)**：无状态核心、带 `ttlMs` 的可缓存 `tools/list`、头部路由——都是为了让工具上下文更便宜、更新鲜
- **[Claude Code 上下文文档](https://code.claude.com/docs/en/memory)**：CLAUDE.md 记忆层级、`/compact`、子 Agent 与 Hooks——被广泛借鉴的实用上下文管理模型
- **[OpenAI Agents API](https://openai.com/index/introducing-the-agents-api)**：将 Codex 背后的智能体 harness 以 API 开放——托管或自建沙箱、MCP 工具与并发子智能体，面向长时运行的智能体
- **[skills（vercel-labs）](https://github.com/vercel-labs/skills)** ⭐36.3k：开放的 agent skills 工具（`npx skills`），跨智能体安装与管理技能
- **[SkillSpector](https://github.com/NVIDIA/SkillSpector)** ⭐21.1k：NVIDIA 的 agent skills 安全扫描器，检测技能目录中的提示注入、数据外泄与供应链风险
- **[ECC](https://github.com/affaan-m/ECC)** ⭐287k：智能体 harness 性能优化系统——面向 Claude Code、Codex、OpenCode、Cursor 的技能、本能、记忆与安全
- **[Harness²](https://github.com/google-research/harness2)** ⭐18.6k：Google Research 的递归式 harness 优化——围绕模型优化提示词、工具、记忆、技能与编排
- **[Strands harness-sdk](https://github.com/strands-agents/harness-sdk)** ⭐8.9k：开源 SDK（Python/TypeScript），端到端构建并控制智能体 harness
- **[durable-harness](https://github.com/vercel-labs/durable-harness)**：在 Vercel Workflow 上持久运行 Claude Code、Codex、OpenCode、Pi 或 fx，记录每一步模型/工具调用，崩溃后可续跑（[博客](https://vercel.com/blog/making-every-harness-durable)）
- **[Raven](https://github.com/EverMind-AI/Raven)**：开源多智能体 harness，把 agent loop 拆成 Memory、Planning、Capability、Action 四个解耦的策略模块，由实验性的 Curator 逐 Agent 改写、验证通过后才安装；Host Agent 把复杂任务规划为 DAG，分派给内置与第三方 Agent

### Context工程系统与工具包

- **[graphify](https://github.com/Graphify-Labs/graphify)** ⭐134.0k：把代码库连同文档、SQL schema、配置与 PDF 转成可查询的知识图谱，以 `/graphify` 技能接入 Claude Code、Cursor、Codex 与 Gemini CLI
- **[提示工程指南 (Prompt Engineering Guide)](https://github.com/dair-ai/Prompt-Engineering-Guide)** ⭐79.5k：提示工程、上下文工程、RAG 与 AI 智能体的综合指南与资源合集
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)** ⭐64.2k：面向 Claude Code 的元提示、上下文工程与规范驱动开发系统
- **[screenpipe](https://github.com/screenpipe/screenpipe)** ⭐22.0k：源码可用、本地优先的工作上下文层，捕获屏幕、音频、输入、浏览器和会议活动，并通过 MCP、REST 和 CLI 向智能体提供可检索的工作证据
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)** ⭐18.2k：面向上下文工程与多代理架构的 Agent Skills 集合
- **[XERJ](https://github.com/xerj-org/xerj)** ⭐14.2k：面向智能体的本地优先搜索引擎与 MCP 服务器，单个 Rust 程序为代码、文档、日志、CSV/JSONL 与 PDF 建立 BM25、向量与混合检索，替代耗 token 的 grep/sed
- **[Context工程入门](https://github.com/coleam00/context-engineering-intro)** ⭐13.9k：面向 AI 编程助手的上下文工程入门方法论
- **[Context-Engineering（jasontang-ai）](https://github.com/jasontang-ai/Context-Engineering)** ⭐9.3k：受 Karpathy 启发的上下文工程第一性原理手册
- **[GSD-2](https://github.com/gsd-build/gsd-2)** ⭐7.8k：支持智能体长时自主工作的元提示／上下文工程系统
- **[MineContext](https://github.com/volcengine/MineContext)** ⭐5.5k：火山引擎主动式上下文感知 AI 伙伴
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)** ⭐3.8k：深入解析 Claude Code 源码：架构、Agent 循环与上下文工程
- **[Awesome-Context-Engineering（Meirtz）](https://github.com/Meirtz/Awesome-Context-Engineering)** ⭐3.3k：上下文工程综述
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)** ⭐2.9k：自纠错记忆的 Claude Code 工作流，含上下文工程
- **[编程智能体的高级上下文工程](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)** ⭐2.8k：面向编码代理的高级上下文工程
- **[ripwire](https://github.com/redhat-et/ripwire)** ⭐2.5k：「AI 上下文版 ripgrep」——零依赖的 C++23 CLI + MCP 服务器，不用通读仓库即可找到相关代码
- **[Context工程工具包](https://github.com/NeoLabHQ/context-engineering-kit)** ⭐1.8k：上下文工程技能套件
- **[ACE（智能体式上下文工程）](https://github.com/ace-agent/ace)** ⭐1.4k：智能体上下文工程（ACE），让语言代理自我进化
- **[Agent QA](https://github.com/vostride/agent-qa)** ⭐899：源码可用的 QA Agent，在自然语言 Web／移动端测试之间保留范围受控的测试记忆与失败证据
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)** ⭐845：面向大模型应用的上下文工程实战指南（中文）
- **[context-space](https://github.com/context-space/context-space)** ⭐812：从 MCP 与集成出发构建的上下文工程基础设施
- **[AI Badger](https://github.com/PVRLabs/aibadger)** ⭐78：本地优先的工具，为 AI 聊天提取聚焦且节省 token 的代码库上下文，并提供用于代码审查和编程智能体交接的 Agent Skills
- **[ax](https://github.com/Necmttn/ax)**：面向 AI 编程智能体会话、成本、技能、路由与 hooks 的本地遥测和召回图谱
- **[kit](https://github.com/cased/kit)**：面向 AI 开发工具的上下文工程工具包，提供代码库映射、符号抽取与代码搜索
- **[RepoPrompt CE](https://github.com/repoprompt/repoprompt-ce)**：RepoPrompt 社区版，面向 AI 编码 agent 的 macOS 原生上下文工程应用，附带 MCP CLI
- **[DevProjex](https://github.com/Avazbek22/DevProjex)**：通过 GUI、TUI、CLI 和只读 MCP 服务，构建整洁且具备 token 感知的项目上下文，并提供实时预览、MCP 模式强制凭据脱敏、Git 范围和语法感知压缩。
- **[context-engineering（outcomeops）](https://github.com/outcomeops/context-engineering)**：上下文工程的可运行参考实现，含语料、检索、注入、输出、强制执行五大组件，在 Amazon Bedrock 上端到端运行
- **[Project Context Records (PCR)](https://github.com/hyf0/project-context-records)**：一套上下文工程方法论，在仓库内持久化、版本化地存档项目的「元上下文」（缘由、架构、维护者决策），让 AI 协作者继承项目判断力，而非反复重新推导
- **[interview-prep-template](https://github.com/AbhiK189/interview-prep-template)**：三层上下文工程模板（不可变原始素材 → agent 维护的 wiki → 运行手册文件），由 agent 把原始素材合成为可复用的答案、框架与带评分的复盘，并随每次面试持续累积变强
- **[MFS](https://github.com/zilliztech/mfs)**：一个上下文 harness，把代码、文档、聊天、数据库和对象存储统一成一个类文件、可搜索的命名空间（`ls`/`cat`/`grep` + 混合语义检索），让智能体按需增量拉取上下文，而不是一开始全部注入；自托管、本地优先（本地 ONNX 向量 + Milvus，无需 API key）
- **[wiki](https://github.com/plasma-ai/wiki)**：面向智能体的索引式 Markdown 知识库，支持按需渐进检索项目上下文，并提供确定性索引、交叉链接、结构校验，以及并行编辑时的合并处理
- **[Hexis](https://github.com/Bevel-Software/Hexis)**：面向 AI Agent 的 Git 驱动技能、工具与上下文平台，支持 Git 评审流程、基于角色的访问控制、加密密钥存储与远程 MCP 接入
- **[PickySteve](https://github.com/KernelLord/pickysteve)**：编程智能体的技能路由与上下文挑选器——BM25 + 向量混合检索（RRF 融合）、带校准下限的交叉编码器重排和 LLM 评审共同选出合适的技能，再为执行模型组装最小的、带 nonce 边界的上下文包；fail-closed 的 ONNX 提示注入闸门同时扫描请求与每篇检索文档。本地优先（可离线运行于 Ollama qwen3:8b 或任意 OpenAI 兼容端点），提供 MCP 服务器和 OpenAI 兼容代理，已接入 18 种编程智能体（Claude Code、Codex、Cursor、Windsurf 等）
- **[YYLO](https://github.com/yylo-dev/yylo)**：面向编程智能体的命令行编排器——每个任务在专属的分支/worktree 中隔离运行，仅承载范围受限的实现与聚焦测试，并提供类型化的任务、验证、合并与发布就绪边界
- **[ctx-optimize](https://github.com/muthuishere/ctx-optimize)**：面向编程智能体的确定性代码知识图谱——单个静态 Go 二进制将仓库（以及可选的数据库、对象存储、消息队列与 API）索引为本地图谱，智能体一次调用即可从图谱中回答「谁调用了它」「改动它会影响什么」，无需再做 grep 与逐文件阅读；内置面向 Claude Code 与 Codex 的 Agent Skill，且不依赖 LLM、嵌入、数据库或 MCP，也不留存任何凭据
- **[CueParcel](https://github.com/kallist/CueParcel)**：本地优先的浏览器扩展，用于选取网页片段、组合多个来源，并打包成有来源依据的上下文供 AI Agent 使用；支持结构化 TaskSpec 输出，以及 GitHub Issue、GitHub Pull Request 与技术文档适配
- **[Archcore](https://github.com/archcore-ai/archcore)**：面向 AI 编程智能体的 Git 原生上下文工程与规范驱动开发
- **[YYLO Benchmark](https://github.com/yylo-dev/yylo-benchmark)**：面向可信主机的编程智能体评测运行器：每次尝试使用全新仓库并保留输出供独立复评；工作区隔离用于整理上下文，不构成安全沙箱
- **[open-bridge](https://github.com/bks-lab/open-bridge)**：编程智能体在会话开始时读取的纯文本 Git 工作区，通过 CI 限制常驻上下文的字节预算，其余信息按需通过索引加载
- **[Curtain](https://github.com/lukstei/curtain)**：分步技能运行器，仅在进入相应阶段时加载该阶段指令，并支持阶段之间的显式人工审批。
- **[Bandpass](https://github.com/MajidAsghariTabrizi/bandpass)**：面向 AI 智能体的上下文编译器——将记忆、状态、文件与证据编译为任务级、带溯源的 Context Capsule，支持 token 预算、冲突消解、显式未知项与 MCP 工具

### 开发框架

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**：用于context管理的低级编排框架
- **[LangSmith](https://docs.smith.langchain.com/)**：智能体追踪和评估平台
- **[LangMem](https://langchain-ai.github.io/langmem/)**：内存管理抽象

### 记忆与压缩

- **[claude-mem](https://github.com/thedotmack/claude-mem)**：面向 Claude Code 的跨会话持久记忆，自动捕获、压缩并在会话间回注上下文
- **[caveman](https://github.com/JuliusBrussee/caveman)** ⭐115k：爆火的技能 + 代理，让编程智能体用极简语言回答，输出 token 减少约 65%
- **[Hindsight](https://github.com/vectorize-io/hindsight)** ⭐53.2k：会学习的智能体记忆，把经历整合为可复用知识
- **[context-mode](https://github.com/mksglu/context-mode)** ⭐32k：面向编程智能体的上下文窗口优化——沙箱化工具输出（最多减少 98%）、持久化会话记忆，并通过 MCP + hooks 在 17 个平台上强制路由
- **[TencentDB Agent Memory](https://github.com/TencentCloud/TencentDB-Agent-Memory)** ⭐29.9k：团队级智能体记忆中枢，把对话、文档与代码转成可复用的记忆资产（对话记忆、技能、LLM-Wiki、代码）
- **[Beads](https://github.com/gastownhall/beads)** ⭐28.2k：编程智能体的记忆升级——用 git 管理的任务图代替冗长的 markdown 计划
- **[ai-memory](https://github.com/akitaonrails/ai-memory)** ⭐13.1k：面向编程智能体 CLI 的长期记忆，并支持在不同厂商的智能体之间交接
- **[Engram](https://github.com/Gentleman-Programming/engram)** ⭐7.6k：与智能体无关的持久记忆，单个 Go 程序（SQLite + FTS5），提供 MCP 服务器、HTTP API、CLI 与 TUI
- **[dsh-context](https://github.com/bowenliang123/dsh-context)** ⭐3.5k：DeepSeek Harness 上下文洞察插件，提供上下文统计与构成的仪表盘、浏览器和侧边栏
- **[agent-beacon](https://github.com/Asymptote-Labs/agent-beacon)** ⭐2.7k：跨 harness、可自我改进的智能体记忆层
- **[billion-context](https://github.com/ranxianglei/billion-context)** ⭐2.5k：上下文压缩插件，支持在约 100K 的小窗口上进行长达一个月的单一会话
- **[Data Olympus](https://github.com/knaisoma/data-olympus)**：面向编程智能体的受治理项目记忆。智能体可以提议上下文，由人把认可的规则升级生效；检索只返回经过有效期与替代关系检查、仍然有效的知识
- **[GoodMemory](https://github.com/hjqcan/GoodMemory)**：面向 AI 应用与编程智能体的本地优先、可审计记忆层，支持 Codex/Claude Code 主机集成、独立 MCP 服务、TypeScript/Python 客户端，以及 SQLite 或 Postgres 存储
- **[LeanCTX (lean-ctx)](https://github.com/yvgude/lean-ctx)**：面向 AI agent 的上下文智能层——单个本地 Rust 二进制决定 agent 读取、记忆与保存什么；token 减少 60–90%，提供 76 个 MCP 工具，本地优先
- **[headroom](https://github.com/chopratejas/headroom)**：在内容进入 LLM 前压缩工具输出/日志，节省 60-95% 的 token
- **[Accordion](https://github.com/a-Fig/Accordion)**：一个 pi 扩展,将 agent 的整个上下文窗口渲染为实时"地图",并通过可插拔的"conductor"(对各块相关性打分)在后台折叠(可逆地压缩)相关性较低的块,替代有损的全量压缩
- **[Mnemoverse](https://github.com/mnemoverse/mcp-memory-server)**：通过 MCP 为 Claude Code、Cursor、VS Code 和 ChatGPT 提供托管的持久记忆，支持 API Key 或 OAuth；依据被召回记忆是否有帮助调整排序
- **[Letta (MemGPT)](https://github.com/letta-ai/letta)**：构建具备长期记忆的有状态智能体框架
- **[Mem0](https://github.com/mem0ai/mem0)**：面向AI智能体和助手的记忆层
- **[Vestige](https://github.com/samvallad33/vestige)**：“本地优先的记忆 MCP 服务器（单个约 23MB Rust 二进制，数据保留在本地），支持实体/因果检索——通过共享的文件、环境变量、服务、符号关联记忆，回溯出导致后续故障、相似度检索可能遗漏的更早改动”
- **[Tree Ring Memory](https://github.com/TerminallyLazy/Tree-Ring-Memory)**：面向 AI 智能体的生命周期感知、本地优先记忆框架，支持 recall、audit、forgetting、consolidation，以及项目级 `.tree-ring` 存储
- **[LLMLingua](https://github.com/microsoft/LLMLingua)**：prompt压缩，加速并降低LLM推理成本
- **[LLMSlim](https://github.com/Thanatos9404/llmslim)**：开源（MIT）的 Python 上下文压缩库，面向 RAG 与智能体：在严格的 token 预算内按查询相关性选择内容单元（词法评分，或可选的 2200 万参数交叉编码器），并公开了与 LLMLingua-2、LongLLMLingua 的可复现对比基准
- **[Cognee](https://github.com/topoteretes/cognee)**：开源 AI 记忆平台，通过自托管知识图谱引擎为智能体提供跨会话的持久长期记忆
- **[Graphiti](https://github.com/getzep/graphiti)**：为 AI 智能体构建实时、时序感知知识图谱的框架（Zep 记忆基础设施的核心引擎）
- **[Supermemory](https://github.com/supermemoryai/supermemory)**：快速、可扩展的记忆与上下文引擎，提供统一 Memory API，支持完全本地运行
- **[skillreaper](https://github.com/thousandflowers/skillreaper)**：基于会话记录统计智能体加载的 context 中真正被调用的比例（技能、MCP 服务器、子智能体、hooks），并可逆地清理未使用的部分
- **[LWC](https://github.com/JanYork/llm-wiki-cli)**：面向编码智能体的主动式、来源可追溯项目记忆，保存不可变原始资料、引用、来源链与原子变更集，提供 SQLite/FTS5 检索、可选文档图与代码图、生命周期 Hook，以及受限的单工具 MCP 接口
- **[billion-context-dsh](https://github.com/Tyan66666/billion-context-dsh)**：面向 DeepSeek Harness 的模型驱动上下文压缩（Active Context Pruning）——由模型决定何时压缩、压缩什么
- **[TekMyra](https://github.com/laconiq-ai/tekmyra)**: 面向 LLM 管道的受治理上下文压缩：输出前检查每个受保护片段（标识符、金额、引用）均被保留，无法保证即拒绝，拒绝也计入公开指标的分母
- **[trace-mcp](https://github.com/nikolai-vysotskyi/trace-mcp)**：本地 MCP 服务器，以预先构建的跨语言代码图替代逐文件读取——框架感知的边会把 PHP 控制器连到它渲染的 Vue 页面、把依赖注入装饰器连到其 provider、把 ORM 调用连到定义该表的迁移文件（81 种语言、87 个框架集成），并提供与代码关联的跨会话决策记忆；在 6 个第三方仓库的 60 个已合并 PR 上，评审所需上下文从中位数 13,595 个输入 token 降到 1,326 个
- **[langstate](https://github.com/hermes-labs-ai/langstate)**：将长对话压缩为可检视的 `[SCAFFOLD STATE]` 消息而非不透明摘要——保留系统消息与最近若干轮原文，`validate()` 会为指定事实返回确定性回执
- **[Caura](https://github.com/caura-ai/caura)**：面向 AI 智能体集群的受治理共享记忆——跨智能体与租户的作用域化召回、信任分级、keystone 策略、审计日志与知识图谱，通过 MCP 暴露
- **[Statewave](https://github.com/smaramwbc/statewave)**：面向 AI 智能体的记忆运行时，将智能体事件编译为结构化记忆，并按主体组装可复现、带来源标注的上下文包，而非依赖查询时检索；基于 Postgres + pgvector 自托管，提供 Python/TypeScript SDK 与 MCP 服务器
- **[Ctxlens](https://github.com/AgentPostmortem/Ctxlens)**：面向 AI 智能体的上下文窗口分析器，展示 token 消耗的去向
- **[tokencut](https://github.com/AgentPostmortem/tokencut)**：度量并削减 LLM 与智能体消息负载的 token 成本
- **[contextpress](https://github.com/Taha-azizi/contextpress)**：面向 LLM 对话、RAG 与智能体流水线的确定性上下文压缩（`pip install contextpress`）——一级为纯 NLP 处理、无需 API Key，可选二级 LLM 后端
- **[toldya](https://github.com/singhlabsdev/toldya)**：从本地 Claude Code 会话记录中找出你反复对 AI 说的纠正，经你逐条确认后写入 CLAUDE.md / AGENTS.md，并统计每条规则添加前后的重复次数
- **[Hyperconsciousness](https://github.com/louis030195/hyperconsciousness)**：处于开发者 alpha 阶段的 Rust 知识存储，支持加密的仅追加记录、设备同步，并通过限定范围、设定有效期的授权控制 MCP 检索
- **[elephant](https://github.com/tonone-ai/elephant)**：为 Claude Code 提供按仓库持久记忆的插件——在仓库根目录维护 `ELEPHANT.md`，会话启动时自动加载，并随代码提交以便团队成员共享同一上下文；另提供跨仓库的全局记忆文件、基于 git 历史的冷启动记忆初始化，以及旧条目压缩
- **[context-bank](https://github.com/kadiresen/context-bank)**：提交到 git 的、有大小上限的编码智能体项目记忆（.ai/），支持按需检索的历史记录、doctor 健康检查以及从 Cline Memory Bank 迁移
- **[Gobstopper](https://github.com/hraness/gobstopper)**：面向 Claude Code、Codex 等编程智能体的本地代理，请求超过 token 阈值时将较早轮次替换为机械摘要，最新轮次原样发送；也可为已保存会话生成更小的副本
- **[Agent-Body](https://github.com/1420079678-ctrl/agent-body)**：面向 DeepSeek Harness 的插件层，按命令意图显影工具 schema，提供确定性反射与空闲期记忆巩固；附无需 API key 的离线演示和可复现的工具 schema token 基准
- **[Context Guru](https://github.com/rossoctl/context-guru)**：provider 无关的上下文引擎，结合 prompt 缓存管理、工具输出过滤与上下文压缩，支持 Claude Code 与 Codex 集成
- **[Dory](https://github.com/tjqscott/dory)**：用相互链接的 Markdown wiki 页面保存项目决策、研究结果和任务；Claude Code 钩子在会话开始时加载索引，并在修改后检查 wiki 结构
- **[Sunstone](https://github.com/space-bacon/sunstone-vscode)**：源码可见（BSL 1.1）的 VS Code 扩展，在本机为你打开的文件夹建立索引，并为模型选择器中的任意模型（包括 Copilot 的模型）提供检索这些文件夹的工具；对于你自己的模型服务器，窗口容纳不下的对话轮次和工具结果会被折叠进同一个本地存储，在与问题最相关时取回，并提供一个 recall 工具用于检索这些内容

- **[ctxprune](https://github.com/riceharvest/ctxprune)**：小型 token 分类模型（类似 LLMLingua-2，支持 CPU/ONNX），删除工具输出、日志和文档中的低价值词语，同时保护 ID、路径与数字（`pip install ctxprune`）
- **[mu](https://github.com/qybaihe/mu)**：基于 pi 的编程智能体，把上下文决策交给小型判定模型：长工具输出逐块判定，只有当前需要的部分进入上下文，其余存档并留指针；上下文超过阈值时，过时的工具结果替换为一行占位；压缩按段落取舍，不写摘要；失败测试日志中完全重复的内容可无损折叠（作者 7 份最大的真实日志共减少 51% 字符）
- **[akc-cycle](https://github.com/shimo4228/akc-cycle)**：Claude Code 插件，防止配置中的规则与技能不断堆积、逐渐过时；审计技能、规则与智能体定义，为每个文件给出保留、合并或退役的建议及理由，确认前不做任何修改，并将反复出现的建议提升为常驻加载的规则
- **[contexttax](https://github.com/henrikdukefoss/contexttax)**：按智能体统计编程智能体每次请求都会加载的固定上下文（CLAUDE.md/AGENTS.md 及其 `@` 导入、常驻规则、技能描述、MCP 工具 schema），按 token 从大到小排列，并指出哪些部分可以改为按需加载
- **[turntax](https://github.com/TorutDev/turntax)**：按各编程智能体文档中的加载规则，测算 16 种智能体每轮都会发送的指令文件（AGENTS.md、CLAUDE.md、Cursor/Copilot/Windsurf 规则）的 token 成本，区分常驻与按需加载的文件，并预览安全的修复；提供 CLI、GitHub Action 和只读 MCP 服务器
- **[gknowledge](https://github.com/openapi/gknowledge)**：以原生 GNOME 应用实现的本地优先 Karpathy LLM Wiki：原始资料保持不可变，每次导入只更新相关的 Markdown 页面（YAML frontmatter 记录来源、`index.md` 目录、仅追加的 `log.md`、跨来源合并同名实体），提问时基于编译好的 wiki 给出带引用的回答，而不是重新读取原始文档；内置 MCP 服务器（`search_wiki`、`read_page`、`ask`、`add_source`）让 Claude Code、Codex 或 Cursor 读取并扩充同一个 wiki，离线引擎无需模型或 API key 即可使用（MIT）
- **[Chisle](https://github.com/JayPokale/Chisle)**：适用于 Claude Code、Pi、OpenCode 和 Copilot CLI 的插件（另为九个智能体生成规则文件），在工具输出重新进入上下文之前压缩过长的结果（保留开头和结尾、保留错误行、同一会话内去重；从不改动 Read/Edit/Write 的输出），并加入"上下文节食"规则（先 grep 再读取、分段读取、不重复读取）；在实测的 171 个 Claude Code 会话中，工具输出占上下文的 67.5%，该钩子将符合条件的输出平均缩减约 46%。确定性处理，无依赖，无网络请求
- **[Next Context](https://github.com/ajwann/next-context)**：Claude Code 插件，在输入框上方实时显示上下文窗口：按 `/context` 类别统计的用量、首条消息之前已加载的开销（并标出占用最多的 MCP 服务器）、每轮增长，以及距自动压缩还剩多少轮的预测，并支持自定义警告阈值；完全在本地运行，不发起网络或 API 调用
- **[Off the Wall](https://github.com/toranalytics/off-the-wall)**：Claude Code 与 Codex 插件，保存你纠正智能体的消息，并在它回答前逐字引用最相关的几条，而不是把它们总结成规则；`otw report` 显示你最常纠正的内容，路由规则要求编排者在每次调用子智能体时指定模型。无需 API key，无依赖
- **[agent-files-check](https://github.com/lesshands/agent-files-check)**：检查编程智能体的指令文件（AGENTS.md、CLAUDE.md、GEMINI.md、Cursor、Copilot、Windsurf 与 Cline 规则）中引用的文件和文件夹，报告 git 历史表明已被删除或移动的引用，给出 file:line、对应提交，并在能判断时给出新位置；作为 GitHub Action 运行时，会在 pull request 中标注其引用路径被该改动删除或重命名的指令行
- **[GFT Map](https://github.com/CoralLips/gft-map)**：面向 Claude Code 与 Codex 的本地应用，用可编辑文稿和关系图保存决定、理由与待解决问题，按主题交给下一场会话读取
- **[lean-agent-tools](https://github.com/SirGrokkius/lean-agent-tools)**：14 个单文件、仅依赖 Python 标准库的命令行过滤工具，放在 shell 输出与模型之间：`logdiff` 将日志折叠为带计数的模板（保留错误行），`jsonslim` 只保留指定的 JSON 路径，`htmltext` 在预算内返回网页正文，另有缓存、仅在变化时输出的轮询工具和无依赖的 token 估算器；附每个工具的前后 token 实测数据（o200k）
- **[lcc](https://github.com/lucasmartins-ai/lcc)**：不做摘要地压缩编程智能体会话：已用过的工具输出移入本地内容寻址存储，智能体可按哈希 `recall` 取回原文，用户与助手消息逐字节保留，每次压缩都输出 JSON 报告说明删了什么、为什么；离线运行，提供 CLI、MCP 服务器和 Claude Code 插件
- **[Mnemosyne OS](https://github.com/Mnemosyne-OS/Mnemosyne-Neural-OS)**：面向 Windows、macOS 与 Linux 的桌面记忆应用。被监视的文件夹在用户本机上分别汇入按领域划分的独立记忆库（vault）；检索将向量排序与本地 BM25 关键词通道融合，并由用户决定接入的模型可以读取哪些记忆库；智能体通过 MIT 许可的 MCP 服务器（`npx -y -p @mnemosyne_os/mcp mnemosyne-mcp`）接入。开放核心（open core）：记忆引擎以闭源形式随免费桌面版提供

### 生产工具

- **Claude Code**：自动压缩context管理
- **ChatGPT**：跨会话长期记忆
- **[Puppyone](https://github.com/puppyone-ai/puppyone)**：面向智能体的文件系统式context存储
- **Cursor**：基于规则的context工程
- **Windsurf**：高级代码context检索
- **[OrcaReplay](https://github.com/Continuum-AI-Corp/OrcaReplay)**：逐轮记录智能体实际发出的上下文，并可离线重放
- **[Praxix](https://praxix.app)**：面向 Claude Code 的商业 macOS 应用，按会话精选 context：精确选择每个会话加载的 skills、rules 和 references（预设、项目默认、精简的 CLAUDE.md），在消耗 token 之前先看到完整 context
- **[Aident Loadout](https://aident.ai/)**：托管的 MCP 服务器、CLI 与 Skill，用于管理智能体的工具 context：智能体按意图搜索动作，只为实际执行的动作读取实时 schema，可直接使用 400+ 专家编写的 Skills；API 密钥保存在 Aident Vault 中，不进入模型 context

## 💡 专家观点

### 行业领袖

**Andrej Karpathy** 

```
+1 支持“上下文工程”优于“提示工程”。

人们通常把“提示”理解为你在日常使用大语言模型（LLM）时给出的简短任务描述。而在每一个工业级的LLM应用中，“上下文工程”才是真正的精细艺术与科学——它是在上下文窗口中为下一步填充恰到好处的信息。说它是科学，是因为做好这件事需要任务描述与解释、few-shot 示例、RAG、相关（可能是多模态的）数据、工具、状态与历史、信息压缩……信息太少或形式不对，LLM就无法获得最佳表现所需的上下文；信息太多或太无关，LLM的成本会上升，性能反而可能下降。要做好这件事绝非易事。而说它是艺术，则是因为这背后有着对LLM“心理”与人类精神的直觉把握。

除了上下文工程本身，LLM应用还需要：
 • 恰当地将问题拆解为控制流
 • 恰当地填充上下文窗口
 • 调度合适类型和能力的LLM
 • 处理生成-验证的UI/UX流程
 • 还有很多其他工作——安全防护、评估、并行处理、预取……

所以，上下文工程只是正在兴起的一整套复杂软件体系中的一小部分，这套体系将单次LLM调用（以及更多内容）协调成完整的LLM应用。“ChatGPT封装器”这个说法已经过时，而且真的非常不准确。
```

### 关键原则

1. **KV-Cache优化**：设计以提高缓存命中率，降低延迟和成本
2. **仅追加Context**：避免修改之前的context以保持缓存有效性
3. **外部内存**：使用文件系统和数据库作为扩展context存储
4. **错误保留**：保留失败轨迹用于模型学习和适应
5. **多样性胜过统一性**：避免导致模型漂移的重复模式
6. **渐进式披露**：工具与技能定义平时不进入静态上下文，需要时再加载完整 schema/指令
7. **度量 Harness**：跟踪缓存命中率、每轮固定上下文与单任务 token 成本；同一模型在不同 harness 下表现可相差一倍
8. **把摘要当作不可信输入**：压缩是有损的，还可能夹带注入指令——保留可恢复的原始历史，并审计被丢弃的内容

## 🔗 模型Context协议 (MCP)

### Context7 MCP服务器
**为LLM和AI代码编辑器提供最新的代码文档**
- 🔗 仓库：[upstash/context7](https://github.com/upstash/context7)
- 🎯 开发工作流的实时代码context
- 🚀 与流行AI编程助手集成

### MCP生态系统
- **[模型Context协议](https://modelcontextprotocol.io/introduction)**：标准化context共享
- **[文件系统服务器](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)**：基于文件的context管理
- **[installed-api](https://github.com/ajwann/installed-api)**：MCP 服务器（以 Claude Code 插件形式提供），根据项目中实际安装的包回答 Python 和 TypeScript API 问题，而不是依赖模型记忆或网页文档：从项目自身的解释器获取签名、文档字符串、源码和版本，从 `node_modules` 读取 `.d.ts` 声明，只返回有长度上限的片段而非整个文件；仅使用 Python 标准库，不发起网络请求
- **工具集成**：工具间无缝context流动

## 🤝 贡献指南

我们欢迎贡献！请查看我们的[贡献指南](CONTRIBUTING.md)了解详情。

### 如何贡献

1. **添加资源**：提交与context工程相关的论文、工具或文章
2. **改进翻译**：帮助将内容翻译成不同语言
3. **分享见解**：贡献专家意见和最佳实践
4. **报告问题**：帮助我们保持准确性和相关性

### 贡献类别

- 📄 研究论文
- 🛠️ 工具与库
- 📖 文章与教程
- 💡 专家见解
- 🔧 实现示例
- 🌐 翻译

## ⭐ Star历史

[![Star History Chart](https://api.star-history.com/svg?repos=yzfly/awesome-context-engineering&type=Date)](https://star-history.com/#yzfly/awesome-context-engineering&Date)

## 📄 许可证

本项目采用[CC0 1.0](LICENSE)。

## 🙏 致谢

特别感谢所有贡献者和推进context工程领域发展的研究社区。

---

**维护者**：[yzfly](https://github.com/yzfly) | **云中江树（微信公众号: 云中江树）**

*如果您觉得这个仓库有帮助，请考虑给它一个⭐！*
