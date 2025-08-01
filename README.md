# Awesome Context Engineering

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![GitHub stars](https://img.shields.io/github/stars/yzfly/awesome-context-engineering.svg?style=social&label=Star)](https://github.com/yzfly/awesome-context-engineering)
[![GitHub forks](https://img.shields.io/github/forks/yzfly/awesome-context-engineering.svg?style=social&label=Fork)](https://github.com/yzfly/awesome-context-engineering)

A curated collection of resources, papers, tools, and best practices for **Context Engineering** in AI agents and Large Language Models (LLMs).

> Context engineering is the art and science of filling the context window with just the right information at each step of an agent's trajectory. 

[中文版本](README_CN.md) | [English](README.md)

## 📚 Table of Contents

- [What is Context Engineering?](#what-is-context-engineering)
- [Featured Articles](#featured-articles)
- [Research Papers](#research-papers)
- [Tools & Projects](#tools--projects)
- [Expert Insights](#expert-insights)
- [Model Context Protocol (MCP)](#model-context-protocol-mcp)
- [Contributing](#contributing)
- [Star History](#star-history)

## What is Context Engineering?

Context Engineering is the systematic optimization of information payloads for Large Language Models (LLMs). It encompasses:

- **Context Retrieval & Generation**: Selecting and creating relevant information
- **Context Processing**: Organizing and structuring context for optimal consumption
- **Context Management**: Handling context windows, memory, and state across interactions
- **Context Compression**: Reducing token usage while preserving essential information
- **Context Isolation**: Separating concerns across different context spaces

## 📖 Featured Articles

### Manus Context Engineering
**Context Engineering for AI Agents: Lessons from Building Manus**
- 📄 Original: [Manus Blog](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
- 🇨🇳 Chinese Translation: [docs/manus/AI智能体的Context工程：构建Manus的经验教训.md](docs/manus/AI智能体的Context工程：构建Manus的经验教训.md)

Key insights from building a production AI agent:
- Design around KV-Cache for performance optimization
- Mask, don't remove tools for better action selection
- Use file system as external context memory
- Manipulate attention through recitation techniques

### LangChain Context Engineering
**Context Engineering for Agents**
- 📄 Original: [LangChain Blog](https://blog.langchain.com/context-engineering-for-agents/)
- 🇨🇳 Chinese Translation: [docs/langchain/智能体的Context工程-中文版.md](docs/langchain/智能体的Context工程-中文版.md)

Comprehensive guide covering four key strategies:
- **Write Context**: Saving information outside context window
- **Select Context**: Pulling relevant information into context
- **Compress Context**: Retaining only necessary tokens
- **Isolate Context**: Splitting context across different spaces

### Claude Code Best Practices
**Claude Code Best Practices**
- 📄 Original: [Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-best-practices)
- 🇨🇳 Chinese Translation: [docs/claudecode/claude-code-best-practices-zh.md](docs/claudecode/claude-code-best-practices-zh.md)

### dbreunig Context Engineering Series
**How Long Contexts Fail and How to Fix Them**
- 📄 Original Part 1: [How Long Contexts Fail](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)
- 🇨🇳 Chinese Translation: [docs/dbreunig/长上下文的失效原理及解决方案.md](docs/dbreunig/长上下文的失效原理及解决方案.md)
- 📄 Original Part 2: [How to Fix Your Context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)
- 🇨🇳 Chinese Translation: [docs/dbreunig/上下文修复的实用指南.md](docs/dbreunig/上下文修复的实用指南.md)

Deep dive into context failure modes and management strategies:
- Context poisoning, distraction, confusion, and clash patterns
- RAG, tool loadout, context quarantine, pruning, summarization, and offloading techniques


## 📑 Research Papers

### Survey Papers

**A Survey of Context Engineering for Large Language Models**
- 📄 arXiv: [2507.13334](https://arxiv.org/abs/2507.13334)
- 📊 Comprehensive analysis of 1400+ research papers
- 🎯 Establishes formal taxonomy of context engineering components

> *The performance of Large Language Models (LLMs) is fundamentally determined by the contextual information provided during inference. This survey introduces Context Engineering, a formal discipline that transcends simple prompt design to encompass the systematic optimization of information payloads for LLMs.*

### Core Research Areas

- **Memory Systems**: [Reflexion](https://arxiv.org/abs/2303.11366), [Generative Agents](https://ar5iv.labs.arxiv.org/html/2304.03442)
- **Retrieval-Augmented Generation**: [RAG Survey](https://github.com/langchain-ai/rag-from-scratch)
- **Tool Integration**: [Tool Selection](https://arxiv.org/abs/2410.14594), [BigTool](https://arxiv.org/abs/2505.03275)
- **Context Compression**: [Recursive Summarization](https://arxiv.org/pdf/2308.15022), [Context Pruning](https://arxiv.org/abs/2501.16214)

## 🛠️ Tools & Projects

### Comprehensive Resources

1. **[Awesome Context Engineering Survey](https://github.com/Meirtz/Awesome-Context-Engineering)**
   - Comprehensive survey of context engineering techniques
   - Methodologies and applications overview
   - Academic research focus

2. **[Context Engineering Intro](https://github.com/coleam00/context-engineering-intro)**
   - Practical guide for AI coding assistants
   - Claude Code centered approach
   - Hands-on implementation strategies

### Development Frameworks

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**: Low-level orchestration framework for context management
- **[LangSmith](https://docs.smith.langchain.com/)**: Agent tracing and evaluation platform
- **[LangMem](https://langchain-ai.github.io/langmem/)**: Memory management abstractions

### Production Tools

- **Claude Code**: Auto-compact context management
- **Cursor**: Rules-based context engineering

## 💡 Expert Insights

### Industry Leaders

**Andrej Karpathy** (OpenAI)
~~~
+1 for "context engineering" over "prompt engineering".

People associate prompts with short task descriptions you'd give an LLM in your day-to-day use. When in every industrial-strength LLM app, context engineering is the delicate art and science of filling the context window with just the right information for the next step. Science because doing this right involves task descriptions and explanations, few shot examples, RAG, related (possibly multimodal) data, tools, state and history, compacting... Too little or of the wrong form and the LLM doesn't have the right context for optimal performance. Too much or too irrelevant and the LLM costs might go up and performance might come down. Doing this well is highly non-trivial. And art because of the guiding intuition around LLM psychology of people spirits.

On top of context engineering itself, an LLM app has to:
- break up problems just right into control flows
- pack the context windows just right
- dispatch calls to LLMs of the right kind and capability
- handle generation-verification UIUX flows
- a lot more - guardrails, security, evals, parallelism, prefetching, ...

So context engineering is just one small piece of an emerging thick layer of non-trivial software that coordinates individual LLM calls (and a lot more) into full LLM apps. The term "ChatGPT wrapper" is tired and really, really wrong.
~~~

### Key Principles

1. **KV-Cache Optimization**: Design for cache hit rates to reduce latency and cost
2. **Append-Only Context**: Avoid modifying previous context to maintain cache validity
3. **External Memory**: Use file systems and databases as extended context storage
4. **Error Preservation**: Keep failure traces for model learning and adaptation
5. **Diversity Over Uniformity**: Avoid repetitive patterns that lead to model drift

## 🔗 Model Context Protocol (MCP)

### Context7 MCP Server
**Up-to-date code documentation for LLMs and AI code editors**
- 🔗 Repository: [upstash/context7](https://github.com/upstash/context7)
- 🎯 Real-time code context for development workflows
- 🚀 Integration with popular AI coding assistants

### MCP Ecosystem
- **[Model Context Protocol](https://modelcontextprotocol.io/introduction)**: Standardized context sharing
- **[Filesystem Server](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)**: File-based context management
- **Tool Integration**: Seamless context flow between tools

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### How to Contribute

1. **Add Resources**: Submit papers, tools, or articles related to context engineering
2. **Improve Translations**: Help translate content to different languages
3. **Share Insights**: Contribute expert opinions and best practices
4. **Report Issues**: Help us maintain accuracy and relevance

### Contribution Categories

- 📄 Research Papers
- 🛠️ Tools & Libraries
- 📖 Articles & Tutorials
- 💡 Expert Insights
- 🔧 Implementation Examples
- 🌐 Translations

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=yzfly/awesome-context-engineering&type=Date)](https://star-history.com/#yzfly/awesome-context-engineering&Date)

## 📄 License

This project is licensed under the [MIT License](LICENSE).

## 🙏 Acknowledgments

Special thanks to all contributors and the research community advancing the field of context engineering.

---

**Maintained by**: [yzfly](https://github.com/yzfly) | **云中江树**

*If you find this repository helpful, please consider giving it a ⭐!*

