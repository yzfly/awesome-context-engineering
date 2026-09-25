# Awesome Context Engineering

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![GitHub stars](https://img.shields.io/github/stars/yzfly/awesome-context-engineering.svg?style=social&label=Star)](https://github.com/yzfly/awesome-context-engineering)
[![GitHub forks](https://img.shields.io/github/forks/yzfly/awesome-context-engineering.svg?style=social&label=Fork)](https://github.com/yzfly/awesome-context-engineering)

精心整理的**Context工程**资源集合，涵盖AI智能体和大语言模型(LLM)的相关资源、论文、工具和最佳实践。

> Context工程是在智能体轨迹的每一步中，用恰当的信息填充context窗口的艺术和科学。

[中文版本](README_CN.md) | [English](README.md)

## 📚 目录

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

### LangChain Context工程
**智能体的Context工程**
- 📄 原文：[LangChain博客](https://blog.langchain.com/context-engineering-for-agents/)
- 🇨🇳 中文翻译：[智能体的Context工程-中文版.md](docs/langchain/智能体的Context工程-中文版.md)

涵盖四大关键策略的综合指南：
- **写入Context**：将信息保存在context窗口之外
- **选择Context**：将相关信息拉入context
- **压缩Context**：仅保留必要的token
- **隔离Context**：在不同空间中分割context


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

## 📑 研究论文

### 综述论文

**大语言模型Context工程综述**
- 📄 arXiv：[2507.13334](https://arxiv.org/abs/2507.13334)
- 📊 对1400+研究论文的综合分析
- 🎯 建立了context工程组件的正式分类法

> *大语言模型(LLM)的性能从根本上由推理过程中提供的上下文信息决定。本综述介绍了Context工程，这是一门超越简单prompt设计的正式学科，涵盖了对LLM信息负载的系统性优化。*

**Context Engineering 2.0：上下文工程的语境**
- 📄 arXiv：[2510.26493](https://arxiv.org/abs/2510.26493)
- 🎯 将上下文工程追溯二十余年，系统性给出定义，并梳理从人机交互到人-智能体范式的设计原则

**Agentic Context Engineering：面向自我改进语言模型的演化式上下文**
- 📄 arXiv：[2510.04618](https://arxiv.org/abs/2510.04618)
- 🎯 将 context 视为可演化的 playbook（生成器/反思器/整理器），避免 context 坍塌；智能体任务 +10.6%、金融 +8.6%（ICLR 2026）

**AI 智能体时代的记忆：综述**
- 📄 arXiv：[2512.13564](https://arxiv.org/abs/2512.13564)
- 🎯 提出"形态-功能-动态"多维记忆分类法，并厘清 agent memory 与 RAG、上下文工程的区别

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
- **[Agent Skills 开放标准](https://agentskills.io/)**：以 `SKILL.md` 目录通过渐进式披露加载（名称/描述 → 完整指令 → 附带文件），已被 40+ Agent 采纳（Claude Code、Codex、Gemini CLI、Cursor、OpenCode、dsh…），是*程序性*上下文的可移植单元
- **[MCP 2026-07-28 规范](https://blog.modelcontextprotocol.io/posts/2026-07-28/)**：无状态核心、带 `ttlMs` 的可缓存 `tools/list`、头部路由——都是为了让工具上下文更便宜、更新鲜
- **[Claude Code 上下文文档](https://code.claude.com/docs/en/memory)**：CLAUDE.md 记忆层级、`/compact`、子 Agent 与 Hooks——被广泛借鉴的实用上下文管理模型

### Context工程系统与工具包

- **[提示工程指南 (Prompt Engineering Guide)](https://github.com/dair-ai/Prompt-Engineering-Guide)** ⭐76.1k：提示工程、上下文工程、RAG 与 AI 智能体的综合指南与资源合集
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)** ⭐64.5k：面向 Claude Code 的元提示、上下文工程与规范驱动开发系统
- **[screenpipe](https://github.com/screenpipe/screenpipe)** ⭐19.9k：源码可用、本地优先的工作上下文层，捕获屏幕、音频、输入、浏览器和会议活动，并通过 MCP、REST 和 CLI 向智能体提供可检索的工作证据
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)** ⭐16.7k：面向上下文工程与多代理架构的 Agent Skills 集合
- **[Context工程入门](https://github.com/coleam00/context-engineering-intro)** ⭐13.5k：面向 AI 编程助手的上下文工程入门方法论
- **[Context-Engineering（jasontang-ai）](https://github.com/jasontang-ai/Context-Engineering)** ⭐9.1k：受 Karpathy 启发的上下文工程第一性原理手册
- **[GSD-2](https://github.com/gsd-build/gsd-2)** ⭐7.7k：支持智能体长时自主工作的元提示／上下文工程系统
- **[MineContext](https://github.com/volcengine/MineContext)** ⭐5.4k：火山引擎主动式上下文感知 AI 伙伴
- **[Awesome-Context-Engineering（Meirtz）](https://github.com/Meirtz/Awesome-Context-Engineering)** ⭐3.2k：上下文工程综述
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)** ⭐2.7k：深入解析 Claude Code 源码：架构、Agent 循环与上下文工程
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)** ⭐2.3k：自纠错记忆的 Claude Code 工作流，含上下文工程
- **[编程智能体的高级上下文工程](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)** ⭐1.7k：面向编码代理的高级上下文工程
- **[Context工程工具包](https://github.com/NeoLabHQ/context-engineering-kit)** ⭐1.2k：上下文工程技能套件
- **[ACE（智能体式上下文工程）](https://github.com/ace-agent/ace)** ⭐1.2k：智能体上下文工程（ACE），让语言代理自我进化
- **[context-space](https://github.com/context-space/context-space)** ⭐812：从 MCP 与集成出发构建的上下文工程基础设施
- **[Agent QA](https://github.com/vostride/agent-qa)** ⭐772：源码可用的 QA Agent，在自然语言 Web／移动端测试之间保留范围受控的测试记忆与失败证据
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)** ⭐708：面向大模型应用的上下文工程实战指南（中文）
- **[AI Badger](https://github.com/PVRLabs/aibadger)** ⭐21：本地优先的工具，为 AI 聊天提取聚焦且节省 token 的代码库上下文，并提供用于代码审查和编程智能体交接的 Agent Skills
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)**：面向Claude Code的元提示与规范驱动开发／上下文工程系统
- **[GSD-2](https://github.com/gsd-build/gsd-2)**：支持智能体长时自主工作的元提示／上下文工程系统
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)**：深入解析 Claude Code 源码：架构、Agent 循环与上下文工程
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)**：面向上下文工程与多智能体架构的 Agent Skills 合集
- **[编程智能体的高级上下文工程](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)**：面向编码智能体的高级上下文工程技术
- **[ax](https://github.com/Necmttn/ax)**：面向 AI 编程智能体会话、成本、技能、路由与 hooks 的本地遥测和召回图谱
- **[ACE（智能体式上下文工程）](https://github.com/ace-agent/ace)**：用智能体式上下文工程进化语言智能体
- **[Context工程工具包](https://github.com/NeoLabHQ/context-engineering-kit)**：上下文工程工具包
- **[MineContext](https://github.com/volcengine/MineContext)**：火山引擎主动式上下文感知 AI 伙伴
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)**：让 Claude Code 从纠错中学习的自我修正记忆系统，涵盖上下文工程、并行 worktree、agent 团队与 17 个 skills
- **[kit](https://github.com/cased/kit)**：面向 AI 开发工具的上下文工程工具包，提供代码库映射、符号抽取与代码搜索
- **[RepoPrompt CE](https://github.com/repoprompt/repoprompt-ce)**：RepoPrompt 社区版，面向 AI 编码 agent 的 macOS 原生上下文工程应用，附带 MCP CLI
- **[DevProjex](https://github.com/Avazbek22/DevProjex)**：通过 GUI、TUI、CLI 和只读 MCP 服务，构建整洁且具备 token 感知的项目上下文，并提供实时预览、MCP 模式强制凭据脱敏、Git 范围和语法感知压缩。
- **[context-engineering（outcomeops）](https://github.com/outcomeops/context-engineering)**：上下文工程的可运行参考实现，含语料、检索、注入、输出、强制执行五大组件，在 Amazon Bedrock 上端到端运行
- **[Project Context Records (PCR)](https://github.com/hyf0/project-context-records)**：一套上下文工程方法论，在仓库内持久化、版本化地存档项目的「元上下文」（缘由、架构、维护者决策），让 AI 协作者继承项目判断力，而非反复重新推导
- **[interview-prep-template](https://github.com/AbhiK189/interview-prep-template)**：三层上下文工程模板（不可变原始素材 → agent 维护的 wiki → 运行手册文件），由 agent 把原始素材合成为可复用的答案、框架与带评分的复盘，并随每次面试持续累积变强
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)**：面向大模型应用的上下文工程实战指南（中文）
- **[wiki](https://github.com/plasma-ai/wiki)**：面向智能体的索引式 Markdown 知识库，支持按需渐进检索项目上下文，并提供确定性索引、交叉链接、结构校验，以及并行编辑时的合并处理
- **[Hexis](https://github.com/Bevel-Software/Hexis)**：面向 AI Agent 的 Git 驱动技能、工具与上下文平台，支持 Git 评审流程、基于角色的访问控制、加密密钥存储与远程 MCP 接入
- **[YYLO](https://github.com/yylo-dev/yylo)**：面向编程智能体的命令行编排器——每个任务在专属的分支/worktree 中隔离运行，仅承载范围受限的实现与聚焦测试，并提供类型化的任务、验证、合并与发布就绪边界
- **[ctx-optimize](https://github.com/muthuishere/ctx-optimize)**：面向编程智能体的确定性代码知识图谱——单个静态 Go 二进制将仓库（以及可选的数据库、对象存储、消息队列与 API）索引为本地图谱，智能体一次调用即可从图谱中回答「谁调用了它」「改动它会影响什么」，无需再做 grep 与逐文件阅读；内置面向 Claude Code 与 Codex 的 Agent Skill，且不依赖 LLM、嵌入、数据库或 MCP，也不留存任何凭据
- **[CueParcel](https://github.com/kallist/CueParcel)**：本地优先的浏览器扩展，用于选取网页片段、组合多个来源，并打包成有来源依据的上下文供 AI Agent 使用；支持结构化 TaskSpec 输出，以及 GitHub Issue、GitHub Pull Request 与技术文档适配

### 开发框架

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**：用于context管理的低级编排框架
- **[LangSmith](https://docs.smith.langchain.com/)**：智能体追踪和评估平台
- **[LangMem](https://langchain-ai.github.io/langmem/)**：内存管理抽象

### 记忆与压缩

- **[claude-mem](https://github.com/thedotmack/claude-mem)**：面向 Claude Code 的跨会话持久记忆，自动捕获、压缩并在会话间回注上下文
- **[GoodMemory](https://github.com/hjqcan/GoodMemory)**：面向 AI 应用与编程智能体的本地优先、可审计记忆层，支持 Codex/Claude Code 主机集成、独立 MCP 服务、TypeScript/Python 客户端，以及 SQLite 或 Postgres 存储
- **[LeanCTX (lean-ctx)](https://github.com/yvgude/lean-ctx)**：面向 AI agent 的上下文智能层——单个本地 Rust 二进制决定 agent 读取、记忆与保存什么；token 减少 60–90%，提供 76 个 MCP 工具，本地优先
- **[headroom](https://github.com/chopratejas/headroom)**：在内容进入 LLM 前压缩工具输出/日志，节省 60-95% 的 token
- **[Accordion](https://github.com/a-Fig/Accordion)**：一个 pi 扩展,将 agent 的整个上下文窗口渲染为实时"地图",并通过可插拔的"conductor"(对各块相关性打分)在后台折叠(可逆地压缩)相关性较低的块,替代有损的全量压缩
- **[Letta (MemGPT)](https://github.com/letta-ai/letta)**：构建具备长期记忆的有状态智能体框架
- **[Mem0](https://github.com/mem0ai/mem0)**：面向AI智能体和助手的记忆层
- **[Vestige](https://github.com/samvallad33/vestige)**：“本地优先的记忆 MCP 服务器（单个约 23MB Rust 二进制，数据保留在本地），支持实体/因果检索——通过共享的文件、环境变量、服务、符号关联记忆，回溯出导致后续故障、相似度检索可能遗漏的更早改动”
- **[Tree Ring Memory](https://github.com/TerminallyLazy/Tree-Ring-Memory)**：面向 AI 智能体的生命周期感知、本地优先记忆框架，支持 recall、audit、forgetting、consolidation，以及项目级 `.tree-ring` 存储
- **[LLMLingua](https://github.com/microsoft/LLMLingua)**：prompt压缩，加速并降低LLM推理成本
- **[Cognee](https://github.com/topoteretes/cognee)**：开源 AI 记忆平台，通过自托管知识图谱引擎为智能体提供跨会话的持久长期记忆
- **[Graphiti](https://github.com/getzep/graphiti)**：为 AI 智能体构建实时、时序感知知识图谱的框架（Zep 记忆基础设施的核心引擎）
- **[Supermemory](https://github.com/supermemoryai/supermemory)**：快速、可扩展的记忆与上下文引擎，提供统一 Memory API，支持完全本地运行
- **[skillreaper](https://github.com/thousandflowers/skillreaper)**：基于会话记录统计智能体加载的 context 中真正被调用的比例（技能、MCP 服务器、子智能体、hooks），并可逆地清理未使用的部分
- **[LWC](https://github.com/JanYork/llm-wiki-cli)**：面向编码智能体的主动式、来源可追溯项目记忆，保存不可变原始资料、引用、来源链与原子变更集，提供 SQLite/FTS5 检索、可选文档图与代码图、生命周期 Hook，以及受限的单工具 MCP 接口
- **[billion-context-dsh](https://github.com/Tyan66666/billion-context-dsh)**：面向 DeepSeek Harness 的模型驱动上下文压缩（Active Context Pruning）——由模型决定何时压缩、压缩什么
- **[TekMyra](https://github.com/laconiq-ai/tekmyra)**: 面向 LLM 管道的受治理上下文压缩：输出前验证每个受保护片段恰好出现一次，无法验证即拒绝，拒绝也计入公开指标的分母
- **[trace-mcp](https://github.com/nikolai-vysotskyi/trace-mcp)**：本地 MCP 服务器，以预先构建的跨语言代码图替代逐文件读取——框架感知的边会把 PHP 控制器连到它渲染的 Vue 页面、把依赖注入装饰器连到其 provider、把 ORM 调用连到定义该表的迁移文件（81 种语言、87 个框架集成），并提供与代码关联的跨会话决策记忆；在 6 个第三方仓库的 60 个已合并 PR 上，评审所需上下文从中位数 13,595 个输入 token 降到 1,326 个
- **[langstate](https://github.com/hermes-labs-ai/langstate)**：将长对话压缩为可检视的 `[SCAFFOLD STATE]` 消息而非不透明摘要——保留系统消息与最近若干轮原文，`validate()` 会为指定事实返回确定性回执
- **[Caura](https://github.com/caura-ai/caura)**：面向 AI 智能体集群的受治理共享记忆——跨智能体与租户的作用域化召回、信任分级、keystone 策略、审计日志与知识图谱，通过 MCP 暴露
- **[Ctxlens](https://github.com/AgentPostmortem/Ctxlens)**：面向 AI 智能体的上下文窗口分析器，展示 token 消耗的去向
- **[tokencut](https://github.com/AgentPostmortem/tokencut)**：度量并削减 LLM 与智能体消息负载的 token 成本
- **[contextpress](https://github.com/Taha-azizi/contextpress)**：面向 LLM 对话、RAG 与智能体流水线的确定性上下文压缩（`pip install contextpress`）——一级为纯 NLP 处理、无需 API Key，可选二级 LLM 后端
- **[contextdiet](https://github.com/devilking7x/contextdiet)**：本地优先的 AI 智能体上下文窗口分析器——支持 CLAUDE.md、skill 文件与 `.mcp.json` 的真实 BPE token 统计、按大小排序的逐文件明细、带 20% 规则的窗口占用比仪表、按影响排序的裁剪建议，以及裁剪前后节省模拟器。含在线演示。

### 生产工具

- **Claude Code**：自动压缩context管理
- **ChatGPT**：跨会话长期记忆
- **[Puppyone](https://github.com/puppyone-ai/puppyone)**：面向智能体的文件系统式context存储
- **Cursor**：基于规则的context工程
- **Windsurf**：高级代码context检索
- **[OrcaReplay](https://github.com/Continuum-AI-Corp/OrcaReplay)**：逐轮记录智能体实际发出的上下文，并可离线重放

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

## 🔗 模型Context协议 (MCP)

### Context7 MCP服务器
**为LLM和AI代码编辑器提供最新的代码文档**
- 🔗 仓库：[upstash/context7](https://github.com/upstash/context7)
- 🎯 开发工作流的实时代码context
- 🚀 与流行AI编程助手集成

### MCP生态系统
- **[模型Context协议](https://modelcontextprotocol.io/introduction)**：标准化context共享
- **[文件系统服务器](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)**：基于文件的context管理
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
