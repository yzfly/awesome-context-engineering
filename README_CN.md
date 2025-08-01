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

### Manus Context工程
**AI智能体的Context工程：构建Manus的经验教训**
- 📄 原文：[Manus博客](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
- 🇨🇳 中文翻译：[docs/manus/AI智能体的Context工程：构建Manus的经验教训.md](docs/manus/AI智能体的Context工程：构建Manus的经验教训.md)

构建生产级AI智能体的关键洞察：
- 围绕KV-Cache进行设计以优化性能
- 掩码而非移除工具以改善动作选择
- 使用文件系统作为外部context内存
- 通过复述技术操纵注意力

### LangChain Context工程
**智能体的Context工程**
- 📄 原文：[LangChain博客](https://blog.langchain.com/context-engineering-for-agents/)
- 🇨🇳 中文翻译：[docs/langchain/智能体的Context工程-中文版.md](docs/langchain/智能体的Context工程-中文版.md)

涵盖四大关键策略的综合指南：
- **写入Context**：将信息保存在context窗口之外
- **选择Context**：将相关信息拉入context
- **压缩Context**：仅保留必要的token
- **隔离Context**：在不同空间中分割context

### Claude Code 最佳实践
**Claude Code 最佳实践指南**
- 📄 原文：[Anthropic 官方文档](https://www.anthropic.com/engineering/claude-code-best-practices)
- 🇨🇳 中文翻译：[docs/claudecode/claude-code-best-practices-zh.md](docs/claudecode/claude-code-best-practices-zh.md)

### dbreunig Context工程系列
**长Context的失效原理和解决方案**
- 📄 原文 Part 1：[长Context如何失效](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)
- 🇨🇳 中文翻译：[docs/dbreunig/长上下文的失效原理及解决方案.md](docs/dbreunig/长上下文的失效原理及解决方案.md)
- 📄 原文 Part 2：[如何修复你的Context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)
- 🇨🇳 中文翻译：[docs/dbreunig/上下文修复的实用指南.md](docs/dbreunig/上下文修复的实用指南.md)

深入探讨context失效模式和管理策略：
- Context污染、分散、混乱和冲突模式分析
- RAG、工具配置、context隔离、修剪、总结和卸载技术


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

## 🛠️ 工具与项目

### 综合资源

1. **[Awesome Context工程综述](https://github.com/Meirtz/Awesome-Context-Engineering)**
   - context工程技术的综合调研
   - 方法论和应用概述
   - 学术研究重点

2. **[Context工程入门](https://github.com/coleam00/context-engineering-intro)**
   - AI编程助手的实用指南
   - 以Claude Code为中心的方法
   - 实践实施策略

### 开发框架

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**：用于context管理的低级编排框架
- **[LangSmith](https://docs.smith.langchain.com/)**：智能体追踪和评估平台
- **[LangMem](https://langchain-ai.github.io/langmem/)**：内存管理抽象

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

本项目采用[MIT许可证](LICENSE)。

## 🙏 致谢

特别感谢所有贡献者和推进context工程领域发展的研究社区。

---

**维护者**：[yzfly](https://github.com/yzfly) | **云中江树**

*如果您觉得这个仓库有帮助，请考虑给它一个⭐！*