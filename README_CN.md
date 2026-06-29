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


## 📑 研究论文

### 综述论文

**大语言模型Context工程综述**
- 📄 arXiv：[2507.13334](https://arxiv.org/abs/2507.13334)
- 📊 对1400+研究论文的综合分析
- 🎯 建立了context工程组件的正式分类法

> *大语言模型(LLM)的性能从根本上由推理过程中提供的上下文信息决定。本综述介绍了Context工程，这是一门超越简单prompt设计的正式学科，涵盖了对LLM信息负载的系统性优化。*

### 核心研究领域

- **记忆系统**：[Reflexion](https://arxiv.org/abs/2303.11366)、[生成式智能体](https://ar5iv.labs.arxiv.org/html/2304.03442)
- **检索增强生成**：[RAG综述](https://github.com/langchain-ai/rag-from-scratch)
- **工具集成**：[工具选择](https://arxiv.org/abs/2410.14594)、[BigTool](https://arxiv.org/abs/2505.03275)
- **Context压缩**：[递归摘要](https://arxiv.org/pdf/2308.15022)、[Context修剪](https://arxiv.org/abs/2501.16214)
- **长Context局限**：[Lost in the Middle](https://arxiv.org/abs/2307.03172)

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

### Context工程系统与工具包

- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)** ⭐64.5k：面向 Claude Code 的元提示、上下文工程与规范驱动开发系统
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
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)** ⭐708：面向大模型应用的上下文工程实战指南（中文）
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)**：面向Claude Code的元提示与规范驱动开发／上下文工程系统
- **[GSD-2](https://github.com/gsd-build/gsd-2)**：支持智能体长时自主工作的元提示／上下文工程系统
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)**：深入解析 Claude Code 源码：架构、Agent 循环与上下文工程
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)**：面向上下文工程与多智能体架构的 Agent Skills 合集
- **[编程智能体的高级上下文工程](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)**：面向编码智能体的高级上下文工程技术
- **[ACE（智能体式上下文工程）](https://github.com/ace-agent/ace)**：用智能体式上下文工程进化语言智能体
- **[Context工程工具包](https://github.com/NeoLabHQ/context-engineering-kit)**：上下文工程工具包
- **[MineContext](https://github.com/volcengine/MineContext)**：火山引擎主动式上下文感知 AI 伙伴
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)**：让 Claude Code 从纠错中学习的自我修正记忆系统，涵盖上下文工程、并行 worktree、agent 团队与 17 个 skills
- **[kit](https://github.com/cased/kit)**：面向 AI 开发工具的上下文工程工具包，提供代码库映射、符号抽取与代码搜索
- **[RepoPrompt CE](https://github.com/repoprompt/repoprompt-ce)**：RepoPrompt 社区版，面向 AI 编码 agent 的 macOS 原生上下文工程应用，附带 MCP CLI
- **[context-engineering（outcomeops）](https://github.com/outcomeops/context-engineering)**：上下文工程的可运行参考实现，含语料、检索、注入、输出、强制执行五大组件，在 Amazon Bedrock 上端到端运行
- **[Project Context Records (PCR)](https://github.com/hyf0/project-context-records)**：一套上下文工程方法论，在仓库内持久化、版本化地存档项目的「元上下文」（缘由、架构、维护者决策），让 AI 协作者继承项目判断力，而非反复重新推导
- **[interview-prep-template](https://github.com/AbhiK189/interview-prep-template)**：三层上下文工程模板（不可变原始素材 → agent 维护的 wiki → 运行手册文件），由 agent 把原始素材合成为可复用的答案、框架与带评分的复盘，并随每次面试持续累积变强
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)**：面向大模型应用的上下文工程实战指南（中文）

### 开发框架

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**：用于context管理的低级编排框架
- **[LangSmith](https://docs.smith.langchain.com/)**：智能体追踪和评估平台
- **[LangMem](https://langchain-ai.github.io/langmem/)**：内存管理抽象

### 记忆与压缩

- **[claude-mem](https://github.com/thedotmack/claude-mem)**：面向 Claude Code 的跨会话持久记忆，自动捕获、压缩并在会话间回注上下文
- **[LeanCTX (lean-ctx)](https://github.com/yvgude/lean-ctx)**：面向 AI agent 的上下文智能层——单个本地 Rust 二进制决定 agent 读取、记忆与保存什么；token 减少 60–90%，提供 76 个 MCP 工具，本地优先
- **[headroom](https://github.com/chopratejas/headroom)**：在内容进入 LLM 前压缩工具输出/日志，节省 60-95% 的 token
- **[Accordion](https://github.com/a-Fig/Accordion)**：一个 pi 扩展,将 agent 的整个上下文窗口渲染为实时"地图",并通过可插拔的"conductor"(对各块相关性打分)在后台折叠(可逆地压缩)相关性较低的块,替代有损的全量压缩
- **[Letta (MemGPT)](https://github.com/letta-ai/letta)**：构建具备长期记忆的有状态智能体框架
- **[Mem0](https://github.com/mem0ai/mem0)**：面向AI智能体和助手的记忆层
- **[LLMLingua](https://github.com/microsoft/LLMLingua)**：prompt压缩，加速并降低LLM推理成本

### 生产工具

- **Claude Code**：自动压缩context管理
- **ChatGPT**：跨会话长期记忆
- **Cursor**：基于规则的context工程
- **Windsurf**：高级代码context检索

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