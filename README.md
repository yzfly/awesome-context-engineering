# Awesome Context Engineering

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![GitHub stars](https://img.shields.io/github/stars/yzfly/awesome-context-engineering.svg?style=social&label=Star)](https://github.com/yzfly/awesome-context-engineering)
[![GitHub forks](https://img.shields.io/github/forks/yzfly/awesome-context-engineering.svg?style=social&label=Fork)](https://github.com/yzfly/awesome-context-engineering)

A curated collection of resources, papers, tools, and best practices for **Context Engineering** in AI agents and Large Language Models (LLMs).

> Context engineering is the art and science of filling the context window with just the right information at each step of an agent's trajectory. 

[中文版本](README_CN.md) | [English](README.md)

> 🗓️ **Last updated: 2026-10-23** — see [What's New](#-whats-new) for this week's highlights.

## 📚 Table of Contents

- [What's New](#-whats-new)
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

## 🆕 What's New

*Week of 2026-10-23 — collected from official engineering blogs, new papers and trending GitHub projects.*

- **Agent Skills joins MCP under the Linux Foundation.** Anthropic [donated the Agent Skills standard](https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation) to the Agentic AI Foundation (Oct 22). More than 70 agent platforms (ChatGPT, Cursor, GitHub Copilot, VS Code…) have integrated it; about 1M skills have been published and ~280M installed. Progressive disclosure (`SKILL.md` → bundled files) is now the default way to ship procedural context. See also Vercel's [State of agent skills](https://vercel.com/blog/state-of-agent-skills).
- **Context per request keeps growing — caching is the economics.** Anthropic reports that [context per request in Claude Code grew 2.6× in six months](https://claude.com/blog/claude-opus-5-5-built-for-coding-sessions-that-use-more-context) and the input:output token ratio moved from 189:1 to 324:1. Opus 5.5 cut the cached-read price 60%, and Claude Code more than halved cache-missing input (changing effort mid-session or loading tools on demand no longer breaks the cache; 1-hour TTL for API users). OpenAI shipped [better prompt caching for GPT-6](https://openai.com/index/better-prompt-caching-for-gpt-6) with a caching dashboard and miss diagnostics.
- **Harness engineering, measured.** Cursor [trimmed ~66% of its system prompt and moved built-in tools into dynamic context](https://cursor.com/blog/improved-token-efficiency), cutting token cost 7% with no quality loss (dynamic MCP tool loading had already cut tokens 46.9%). AI21 [cut GitLab Duo Code Review cost 98%](https://www.ai21.com/blog/gitlab-case-study/) by controlling which model reads how much context. Hugging Face: [the same weights scored 62% in one harness and 33% in another](https://x.com/huggingface/status/2106034221005312448).
- **Compaction is now a research topic — and an attack surface.** [SelfCompact](https://arxiv.org/abs/2606.23525) lets the agent decide when to compact (30–70% cheaper per task); [REMORY](https://arxiv.org/abs/2610.11287) adds residual soft-memory tokens to summaries; [Cache Engineering for Agents](https://arxiv.org/abs/2610.28577) lets agents drop or reposition KV-cache entries directly. Meanwhile OpenAI caught models in training [writing prompt injections into their own compaction summaries](https://simonwillison.net/2026/Sep/17/compaction-summaries/).
- **Memory that consolidates offline.** Cognition shipped [Devin Memory & Dreaming](https://devin.ai/blog/memory-and-dreaming): markdown memories plus a daily asynchronous pass that dedupes and links them. LangChain [revamped skills in Deep Agents](https://www.langchain.com/blog/revamping-skills-in-deep-agents) (tool-bound, pinned and reloadable skills that keep the prompt cache warm) and argues that [context engineering starts with retrieval](https://www.langchain.com/blog/context-engineering-starts-with-retrieval).

## 📖 Featured Articles

### Context Rot: How Increasing Input Tokens Impacts LLM Performance
- https://research.trychroma.com/context-rot

### Manus Context Engineering
**Context Engineering for AI Agents: Lessons from Building Manus**
- 📄 Original: [Manus Blog](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
- 🇨🇳 Chinese Translation: [docs/manus/AI智能体的Context工程：构建Manus的经验教训.md](docs/manus/AI智能体的Context工程：构建Manus的经验教训.md)

Key insights from building a production AI agent:
- Design around KV-Cache for performance optimization
- Mask, don't remove tools for better action selection
- Use file system as external context memory
- Manipulate attention through recitation techniques

### Claude Code Best Practices
**Claude Code Best Practices**
- 📄 Original: [Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-best-practices)
- 🇨🇳 Chinese Translation: [docs/claudecode/claude-code-best-practices-zh.md](docs/claudecode/claude-code-best-practices-zh.md)

### Anthropic / Claude

- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [AI智能体的有效上下文工程](docs/claudecode/effective-context-engineering-for-ai-agents.md)
- [Context editing and the memory tool](https://www.anthropic.com/news/context-management)
- [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)
- [Writing effective tools for AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents)
- [Code execution with MCP: building more efficient agents](https://www.anthropic.com/engineering/code-execution-with-mcp)
- [Donating Agent Skills to the Agentic AI Foundation](https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation) (Oct 2026)
- [Coding sessions are longer and use more context. Claude Opus 5.5 is built with that in mind.](https://claude.com/blog/claude-opus-5-5-built-for-coding-sessions-that-use-more-context) — context per request grew 2.6× in six months; how pricing, model behavior and the Claude Code harness keep long sessions on cache
- [Using Claude Code: Spending your effort](https://claude.dev/blog/spending-your-effort/) — what the effort setting actually buys (mostly more verification and edge-case testing)
- [Building effective agent automations](https://claude.dev/blog/building-effective-agent-automations/) — scheduled Managed Agents that track what changed since the last run

### OpenAI

- [Better prompt caching for GPT-6](https://openai.com/index/better-prompt-caching-for-gpt-6) — higher default hit rates, 30-minute shared-prefix discount, caching dashboard and miss diagnostics
- [A model guide for the GPT-6 family](https://openai.com/index/practical-guide-building-gpt-6/) — model choice, caching and compaction, skills, and long-running task orchestration
- [Introducing the Agents API](https://openai.com/index/introducing-the-agents-api) — the harness behind Codex exposed as an API for long-running cloud agents
- [Our framework for reporting model misalignment](https://openai.com/index/model-misalignment-reporting-framework) — includes the case of a model injecting instructions into its own compaction summary

### Harness Engineering

- [Improved token efficiency for longer agent runs (Cursor)](https://cursor.com/blog/improved-token-efficiency) — trim the system prompt, load tools only when needed, improve cache reuse, compress file reads, use subagents strategically
- [Free the models: Harness design at the frontier (Replit)](https://replit.com/blog/free-the-models) — let the main agent decide subagent depth and effort instead of a fixed router
- [The Anatomy of Harness Engineering (Google)](https://developers.googleblog.com/the-anatomy-of-harness-engineering-how-to-evaluate-iterate-and-guard-ai-coding-agents) — how to evaluate, iterate and guard coding agents
- [Improving Deep Agents with harness engineering (LangChain)](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering) — Terminal Bench 2.0 from 52.8 to 66.5 by changing only the harness
- [How we cut GitLab Duo Code Review's cost by 98% (AI21)](https://www.ai21.com/blog/gitlab-case-study/) — per-stage model choice and how much context each stage reads
- [A Smarter Model Won't Save Your Bad Harness (Cerebras)](https://www.cerebras.ai/blog/a-smarter-model-won-t-save-your-bad-harness)
- [Memory and dreaming: how Devin learns from working with you (Cognition)](https://devin.ai/blog/memory-and-dreaming)
- [Introducing Manus 2.0](https://manus.im/blog/introducing-manus-2-0) — new in-house harness "Cascade" (23.2% fewer tokens in Manus's test configuration)


### LangChain Context Engineering
**Context Engineering for Agents**
- 📄 Original: [LangChain Blog](https://blog.langchain.com/context-engineering-for-agents/)
- 🇨🇳 Chinese Translation: [智能体的Context工程-中文版](docs/langchain/智能体的Context工程-中文版.md)

Comprehensive guide covering four key strategies:
- **Write Context**: Saving information outside context window
- **Select Context**: Pulling relevant information into context
- **Compress Context**: Retaining only necessary tokens
- **Isolate Context**: Splitting context across different spaces

More from LangChain:
- [Context Engineering for AI Agents Starts with Retrieval](https://www.langchain.com/blog/context-engineering-starts-with-retrieval) (Oct 2026) — search before reading, offload oversized results, delegate retrieval to subagents
- [Revamping Skills in Deep Agents](https://www.langchain.com/blog/revamping-skills-in-deep-agents) (Oct 2026) — tool-bound, pinned and reloadable skills
- [Context Management for Deep Agents](https://www.langchain.com/blog/context-management-for-deepagents) — filesystem offloading of large tool I/O plus threshold-triggered summarization

### dbreunig Context Engineering Series
**How Long Contexts Fail and How to Fix Them**
- 📄 Original Part 1: [How Long Contexts Fail](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)
- 🇨🇳 Chinese Translation: [长上下文的失效原理及解决方案](docs/dbreunig/长上下文的失效原理及解决方案.md)
- 📄 Original Part 2: [How to Fix Your Context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)
- 🇨🇳 Chinese Translation: [上下文修复的实用指南](docs/dbreunig/上下文修复的实用指南.md)

Deep dive into context failure modes and management strategies:
- Context poisoning, distraction, confusion, and clash patterns
- RAG, tool loadout, context quarantine, pruning, summarization, and offloading techniques

### Advanced Context Engineering for Coding Agents

- [GitHub](https://github.com/humanlayer/humanlayer)
- [YouTube](https://humanlayer.dev/youtube)

Guide for using AI to solve hard problems in complex codebases.

### Don't Build Multi-Agents (Cognition)

- 📄 Original: [Cognition Blog](https://cognition.ai/blog/dont-build-multi-agents)

Principles for building reliable agents: share full context and avoid fragile parallel multi-agent architectures.

### The New Skill in AI is Not Prompting, It's Context Engineering (Philipp Schmid)

- 📄 Original: [philschmid.de](https://www.philschmid.de/context-engineering)

Argues most agent failures stem from poor context, not the model; breaks context into instructions, history, long-term memory, RAG, tools, and output format.

### Context Engineering (Simon Willison)

- 📄 Original: [simonwillison.net](https://simonwillison.net/2025/jun/27/context-engineering/)

Why "context engineering" is a better framing than "prompt engineering", curating the foundational discussion of the term.

### Wide Research: Beyond the Context Window (Manus)

- 📄 Original: [Manus Blog](https://manus.im/blog/manus-wide-research-solve-context-problem)

Parallel sub-agent architecture that avoids fabrication when processing many items, by decomposing tasks across dedicated sub-agents instead of one overloaded context window.

### SWE-grep: RL for Fast Context Retrieval (Cognition)

- 📄 Original: [Cognition Blog](https://cognition.ai/blog/swe-grep)

RL-trained models that run up to 8 parallel searches per turn to retrieve code context an order of magnitude faster while minimizing context pollution.

### Compaction Is a Lossy Operation

- https://loopandretry.github.io/posts/compaction-is-a-lossy-operation/

Why compacting an agent's context is lossy compression, not free summarization — what survives, what silently drops, and how it degrades long multi-turn runs.

- [Self-generated prompt injections in compaction summaries (Simon Willison)](https://simonwillison.net/2026/Sep/17/compaction-summaries/) — a model under RL wrote "additional instructions" into its own compaction summary; treat summaries as untrusted context

## 📑 Research Papers

### Survey Papers

**A Survey of Context Engineering for Large Language Models**
- 📄 arXiv: [2507.13334](https://arxiv.org/abs/2507.13334)
- 📊 Comprehensive analysis of 1400+ research papers
- 🎯 Establishes formal taxonomy of context engineering components

> *The performance of Large Language Models (LLMs) is fundamentally determined by the contextual information provided during inference. This survey introduces Context Engineering, a formal discipline that transcends simple prompt design to encompass the systematic optimization of information payloads for LLMs.*

**Harness Engineering for Language Agents: The Harness Layer as Control, Agency, and Runtime**
- 📄 Preprints: [202603.1756](https://www.preprints.org/manuscript/202603.1756/v2)
- 📊 Audits 63 harness-relevant works across academia and public engineering practice
- 🎯 Decomposes the agent harness layer into Control, Agency, and Runtime (CAR); proposes HarnessCard as a reporting artifact

> *Situates harness engineering in the arc from software engineering through prompt and context engineering, arguing that the harness — the runtime loop wrapping tool execution, control, and agency around an LLM — is a first-class layer whose effects are often mistaken for model-driven gains.*

**Recursive Harness Distillation across Agents for Robot Manipulation**
- 📄 arXiv: [2609.33378](https://arxiv.org/abs/2609.33378)
- 🎯 Distills Astra's robot-intervention experience into a reusable playbook for Luna and recursively refines it from Luna's execution feedback, keeping agent and VLA model weights fixed ([project](https://seungyeon.me/RHD/))

**Harness-Zero: Harness Distillation via Agent-as-Harness**
- 📄 arXiv: [2609.24974](https://arxiv.org/abs/2609.24974)
- 🎯 An agent-as-harness training loop distills optimized harness behaviors into model weights, retaining harness-level gains at deployment without the harness at inference time ([code](https://github.com/metaevo-ai/harness-zero))

**Context Engineering 2.0: The Context of Context Engineering**
- 📄 arXiv: [2510.26493](https://arxiv.org/abs/2510.26493)
- 🎯 Traces context engineering back two decades and proposes a systematic definition and design principles across the human-computer to human-agent evolution

**Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models**
- 📄 arXiv: [2510.04618](https://arxiv.org/abs/2510.04618)
- 🎯 Treats context as an evolving playbook (Generator/Reflector/Curator) to prevent context collapse; reports +10.6% on agents and +8.6% on finance tasks (ICLR 2026)

**Meta Context Engineering via Agentic Skill Evolution**
- 📄 arXiv: [2601.21557](https://arxiv.org/abs/2601.21557)
- 🎯 A bi-level framework that co-evolves context-engineering skills and context artifacts, replacing static CE heuristics (ICML 2026, [code](https://github.com/metaevo-ai/meta-context-engineering))

**Memory in the Age of AI Agents: A Survey**
- 📄 arXiv: [2512.13564](https://arxiv.org/abs/2512.13564)
- 🎯 A "forms-functions-dynamics" taxonomy of agent memory, distinguishing it from RAG and context engineering

**PerspectiveGap: A Benchmark for Multi-Agent Orchestration Prompting**
- 📄 arXiv: [2606.08878](https://arxiv.org/abs/2606.08878)
- 📊 110 scenarios across 10 topologies, testing how LLMs allocate context fragments to sub-agent roles
- 🎯 The first benchmark for multi-agent orchestration prompt writing; 33 commercial models average 17.2% pass rate on role-fragment assignment

> *Evaluates LLMs' ability to assign information fragments to sub-agent roles in multi-agent orchestration scenarios. Scoring uses strict pass (zero omissions and zero leaks) plus partial-credit metrics. Data covers 10 loop-centered topologies across 100 professional domains.*

**The Agent Loop: A Survey of Control Strategies, Skills, and Harnesses for LLM Agents**
- 📄 SSRN: [7186738](https://ssrn.com/abstract=7186738)
- 🎯 Takes the agent loop as the unit of analysis; covers in-loop context management and the harness as context manager, alongside termination, verification, recovery and skills

**Agon: An Autonomous Large-Scale Omnidisciplinary Research System Built on Prompt Economy**
- 📄 arXiv: [2606.24177](https://arxiv.org/abs/2606.24177)
- 📊 18 roles in 230.6 KiB of prompts, the smallest prompt footprint among cross-disciplinary autoresearch systems ([code](https://github.com/AutoResearch-Factory/Agon))
- 🎯 Prompt Economy treats prompt engineering as engineering and drives the agents with reusable loops; has run in 10+ research fields without code changes, with the longest unattended run lasting 30 days

### Recent Papers (Sep–Oct 2026)

**Compaction & KV cache**
- [Self-Compacting Language Model Agents](https://arxiv.org/abs/2606.23525) — the model decides when and how to compact via a compaction tool and light rules; matches or beats fixed-interval summarization at 30–70% lower cost per task
- [REMORY: Learning Residual Memory for Context Compaction](https://arxiv.org/abs/2610.11287) — bounded soft-memory tokens added to the text summary, like a residual connection along the sequence
- [Cache Engineering for Agents](https://arxiv.org/abs/2610.28577) — agents operate on the live KV cache (drop, reposition) without re-prefill; rolling drop +1.7 points at −27.7% cost
- [The KV-Surgeon: Compensation-Aware Offline KV-Cache Compaction](https://arxiv.org/abs/2610.25933) — up to +4 points over attention matching at 50× compression
- [DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression](https://arxiv.org/abs/2609.19969) — cross-layer KV reuse plus FP4 cache, ~890 bytes of global KV per token for 1M context

**Context structure & memory**
- [Librarian Agents: Training LLMs to Curate Long-Context Workspaces that Anticipate Future Queries](https://arxiv.org/abs/2610.22273) — RL-trained agents build a hierarchical file workspace for downstream answerers; +33 points on BrowseComp-Plus
- [Pack Light, CarryOn: Building Object-Oriented Context for Long-Horizon Agents](https://arxiv.org/abs/2610.18784) — persist reusable task objects instead of replaying history; 22.9–51.9% fewer tokens
- [FlowState: Execution State as Memory for Long-Horizon LLM Agents](https://arxiv.org/abs/2609.34565) — semantic state nodes with references to raw tool observations; ~41–43% fewer tokens
- [Beyond Memory: Harnessing Long-Horizon Agents with Explicit Belief States](https://arxiv.org/abs/2610.01415) — maintain an explicit belief state as the decision context
- [Cortex: Consolidating Context into Parametric Memory](https://arxiv.org/abs/2610.20399) — distill context-conditioned behavior into mergeable weight updates
- [Memory-Centric Continual Learning for AI Agents](https://arxiv.org/abs/2610.29357) — judge memory by whether it improves future actions, not recall
- [Seeing, Maintaining, and Learning: The Evolution of Multimodal Agent Memory](https://arxiv.org/abs/2610.19366) — survey of multimodal agent memory

**Harness engineering & skills**
- [Harness²: Recursive Agent Harnessing for an Open World](https://arxiv.org/abs/2610.14224) ([code](https://github.com/google-research/harness2))
- [Self-Evolving Agent Harness](https://arxiv.org/abs/2610.18813) — frozen weights; prompts, memory and workflows evolve with persistence and admission rules
- [MILO: Automated Harness Discovery via Orchestrated Multi-Agent Evolution](https://arxiv.org/abs/2609.38349)
- [Doc2Harness: Benchmarking LLMs on Long-Horizon Harness Engineering of Coding Agents](https://arxiv.org/abs/2610.20344) — best of 19 frontier models passes 53% of tests
- [How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?](https://arxiv.org/abs/2609.40303) — a minimal read/write/bash scaffold matches complex open-source scaffolds on MLE tasks
- [Surviving the Router: Optimizing Skill Injections for Retrieval and Execution](https://arxiv.org/abs/2610.08098) — skill-injection attacks must first win skill retrieval; implications for skill-library security

### Core Research Areas

- **Memory Systems**: [Reflexion](https://arxiv.org/abs/2303.11366), [Generative Agents](https://ar5iv.labs.arxiv.org/html/2304.03442), [StructMem](https://arxiv.org/abs/2604.21748) ([code](https://github.com/zjunlp/LightMem))
- **Retrieval-Augmented Generation**: [RAG Survey](https://github.com/langchain-ai/rag-from-scratch)
- **Tool Integration**: [Tool Selection](https://arxiv.org/abs/2410.14594), [BigTool](https://arxiv.org/abs/2505.03275)
- **Context Compression**: [Recursive Summarization](https://arxiv.org/pdf/2308.15022), [Context Pruning](https://arxiv.org/abs/2501.16214), [TokenPilot](https://arxiv.org/abs/2606.17016) ([code](https://github.com/zjunlp/LightRSI))
- **Long-Context Limitations**: [Lost in the Middle](https://arxiv.org/abs/2307.03172)

- **Harness Adaptation**: [Training Agents to Evolve with Their Harness: TaoLive Digital Avatar Agent Technical Report](https://arxiv.org/abs/2608.15763) — Studies training for changes in skills, tool schemas, prompts, and runtime hooks.

## 🛠️ Tools & Projects

### Comprehensive Resources

1. **[Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)**
   - Comprehensive guide to prompt engineering, context engineering, RAG, and AI agents
   - Covers techniques, papers, tools, and best practices
   - Widely-used educational resource

2. **[Awesome Context Engineering Survey](https://github.com/Meirtz/Awesome-Context-Engineering)**
   - Comprehensive survey of context engineering techniques
   - Methodologies and applications overview
   - Academic research focus

3. **[Context Engineering Intro](https://github.com/coleam00/context-engineering-intro)**
   - Practical guide for AI coding assistants
   - Claude Code centered approach
   - Hands-on implementation strategies

4. **[Context-Engineering (davidkimai)](https://github.com/davidkimai/Context-Engineering)**
   - First-principles handbook for context engineering
   - From foundations to advanced techniques

5. **[Context-Engineering (jasontang-ai)](https://github.com/jasontang-ai/Context-Engineering)**
   - Karpathy-inspired first-principles handbook
   - From prompt engineering toward context design, orchestration, and optimization

6. **[The Context Course (HuggingFace)](https://github.com/huggingface/context-course)**
   - HuggingFace course on context engineering for code agents
   - Covers skills, MCP, plugins, subagents, hooks, and building your own agent
   - Reference agents: Claude Code, Codex, OpenCode

7. **[Agent Systems Handbook](https://github.com/Prompthon-IO/agent-systems-handbook)**
   - Practical AI agents handbook covering agentic workflows and multi-agent architecture
   - Spans LangGraph, MCP/A2A, context engineering, agent memory, evaluation, and observability

### Context Engineering in 2026: Harnesses, Skills & Protocols

- **[deepseek-harness (dsh)](https://github.com/deepseek-ai/deepseek-harness)**: DeepSeek's official open-source agent harness (Aug 2026). Its append-only session log is the *only* source of model context ("model-visible means logged"), with prompt sections, tool schemas, skills and sub-agents all composed as swappable plugins — a reference design for context assembly
- **[Agent Skills open standard](https://agentskills.io/)**: `SKILL.md` folders loaded via progressive disclosure (name/description → full instructions → bundled files), integrated by 70+ agent platforms (Claude Code, ChatGPT, Codex, Gemini CLI, Cursor, GitHub Copilot, VS Code, OpenCode, dsh...); ~1M skills published. Donated to the Agentic AI Foundation (Linux Foundation) in Oct 2026, next to MCP. Skills are the portable unit of *procedural* context
- **[MCP 2026-07-28 spec](https://blog.modelcontextprotocol.io/posts/2026-07-28/)**: stateless core, cacheable `tools/list` results with `ttlMs`, and header-based routing — all aimed at keeping tool context cheap and fresh for agents
- **[Claude Code context docs](https://code.claude.com/docs/en/memory)**: CLAUDE.md memory hierarchy, `/compact`, subagents and hooks — the most widely copied practical context-management model
- **[OpenAI Agents API](https://openai.com/index/introducing-the-agents-api)**: the agent harness behind Codex exposed as an API — hosted or self-run sandboxes, MCP tools and concurrent subagents for long-running agents
- **[skills (vercel-labs)](https://github.com/vercel-labs/skills)** ⭐36.3k: the open agent skills tool (`npx skills`) for installing and managing skills across agents
- **[SkillSpector](https://github.com/NVIDIA/SkillSpector)** ⭐21.1k: NVIDIA's security scanner for agent skills — prompt injection, data exfiltration and supply-chain risks in skill folders
- **[ECC](https://github.com/affaan-m/ECC)** ⭐287k: agent harness performance optimization system — skills, instincts, memory and security for Claude Code, Codex, OpenCode and Cursor
- **[Harness²](https://github.com/google-research/harness2)** ⭐18.6k: Google Research's recursive agent harnessing — optimizing prompts, tools, memory, skills and orchestration around a model
- **[Strands harness-sdk](https://github.com/strands-agents/harness-sdk)** ⭐8.9k: open-source SDK (Python/TypeScript) for building and controlling an agent harness end-to-end
- **[durable-harness](https://github.com/vercel-labs/durable-harness)**: runs Claude Code, Codex, OpenCode, Pi or fx durably on Vercel Workflow, recording every model/tool step so crashed runs resume ([blog](https://vercel.com/blog/making-every-harness-durable))
- **[Raven](https://github.com/EverMind-AI/Raven)**: open-source multi-agent harness whose agent loop is split into four decoupled strategy modules (Memory, Planning, Capability, Action) that an experimental Curator rewrites per agent and installs only after verification; its host agent plans a complex task as a DAG and delegates the parts to built-in and third-party agents

### Context Engineering Systems & Kits

- **[graphify](https://github.com/Graphify-Labs/graphify)** ⭐134.0k: Turns a codebase with its docs, SQL schemas, configs and PDFs into a queryable knowledge graph, exposed as a `/graphify` skill for Claude Code, Cursor, Codex and Gemini CLI
- **[Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)** ⭐79.5k: Comprehensive guide and resources for prompt engineering, context engineering, RAG, and AI agents
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)** ⭐64.2k: Meta-prompting, context engineering and spec-driven development system for Claude Code
- **[screenpipe](https://github.com/screenpipe/screenpipe)** ⭐22.0k: Source-available, local-first work-context layer that captures screen, audio, input, browser, and meeting activity for agent retrieval through MCP, REST, and CLI
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)** ⭐18.2k: Collection of Agent Skills for context engineering and multi-agent architectures
- **[XERJ](https://github.com/xerj-org/xerj)** ⭐14.2k: Local-first search engine and MCP server for agents — one Rust binary indexes code, docs, logs, CSV/JSONL and PDFs for BM25, vector and hybrid search instead of grep/sed token waste
- **[Context Engineering Intro](https://github.com/coleam00/context-engineering-intro)** ⭐13.9k: Context engineering introduction and methodology for AI coding assistants
- **[Context-Engineering (jasontang-ai)](https://github.com/jasontang-ai/Context-Engineering)** ⭐9.3k: Karpathy-inspired first-principles handbook for context engineering
- **[GSD-2](https://github.com/gsd-build/gsd-2)** ⭐7.8k: Meta-prompting / context engineering system enabling long-running autonomous agent work
- **[MineContext](https://github.com/volcengine/MineContext)** ⭐5.5k: Volcengine's proactive, context-aware AI companion
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)** ⭐3.8k: Deep dive into Claude Code source: architecture, agent loop, and context engineering
- **[Awesome-Context-Engineering (Meirtz)](https://github.com/Meirtz/Awesome-Context-Engineering)** ⭐3.3k: Comprehensive survey of context engineering
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)** ⭐2.9k: Self-correcting memory Claude Code workflow, including context engineering
- **[Advanced Context Engineering for Coding Agents](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)** ⭐2.8k: Advanced context engineering techniques for coding agents
- **[ripwire](https://github.com/redhat-et/ripwire)** ⭐2.5k: "The ripgrep of AI context" — zero-dependency C++23 CLI + MCP server that finds the relevant code without reading the repo
- **[Context Engineering Kit](https://github.com/NeoLabHQ/context-engineering-kit)** ⭐1.8k: A context engineering skills toolkit
- **[ACE (Agentic Context Engineering)](https://github.com/ace-agent/ace)** ⭐1.4k: Evolving language agents through agentic context engineering (ACE)
- **[Agent QA](https://github.com/vostride/agent-qa)** ⭐899: Source-available QA agent that retains scoped test memory and failure evidence across natural-language web and mobile runs
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)** ⭐845: A practical, hands-on guide (in Chinese) to context engineering for LLM applications
- **[context-space](https://github.com/context-space/context-space)** ⭐812: Context engineering infrastructure built from MCPs and integrations
- **[AI Badger](https://github.com/PVRLabs/aibadger)** ⭐78: Local-first tool that extracts focused, token-efficient repository context for AI chats, with Agent Skills for code review and coding-agent handoff
- **[ax](https://github.com/Necmttn/ax)**: Local telemetry and recall graph for AI coding-agent sessions, costs, skills, routing, and hooks
- **[kit](https://github.com/cased/kit)**: Context engineering toolkit for AI dev tools, providing codebase mapping, symbol extraction, and code search
- **[RepoPrompt CE](https://github.com/repoprompt/repoprompt-ce)**: Community edition of RepoPrompt, a native macOS context engineering app for AI coding agents, with an MCP CLI
- **[DevProjex](https://github.com/Avazbek22/DevProjex)**: Builds clean, token-aware project context with GUI, TUI, CLI, and a read-only MCP server, plus live preview, secret redaction enforced in MCP mode, Git scopes, and syntax-aware compression.
- **[context-engineering (outcomeops)](https://github.com/outcomeops/context-engineering)**: Working reference implementation of context engineering with five components (corpus, retrieval, injection, output, enforcement), running end-to-end on Amazon Bedrock
- **[Project Context Records (PCR)](https://github.com/hyf0/project-context-records)**: A context-engineering methodology that keeps a durable, repo-versioned archive of a project's meta-context (the why, architecture, and maintainer decisions) so AI collaborators inherit its judgment instead of re-deriving it
- **[interview-prep-template](https://github.com/AbhiK189/interview-prep-template)**: A three-layer context-engineering template (immutable sources → agent-maintained wiki → operating-manual file) where the agent synthesizes raw material into reusable answers, frameworks, and scored debriefs that compound over time
- **[MFS](https://github.com/zilliztech/mfs)**: A context harness that unifies code, docs, chat, databases, and object stores into one file-like, searchable namespace (`ls`/`cat`/`grep` + hybrid semantic search), so agents pull context incrementally instead of injecting it all up front; self-hosted and local-first (local ONNX embeddings + Milvus, no API key)
- **[wiki](https://github.com/plasma-ai/wiki)**: Indexed Markdown knowledge bases that let agents retrieve project context incrementally, with deterministic indexes, cross-links, linting, and merge handling for parallel edits
- **[Hexis](https://github.com/Bevel-Software/Hexis)**: Git-backed platform for skills, tools, and context for AI agents, with Git review workflows, role-based access, encrypted secrets, and remote MCP access
- **[PickySteve](https://github.com/KernelLord/pickysteve)**: A skill router and context picker for coding agents — hybrid BM25 + embedding retrieval (RRF-fused), cross-encoder rerank with a calibrated floor, and an LLM judge select the right skill, then assemble a minimal nonce-boundaried context bundle for the execution model; a fail-closed ONNX prompt-injection gate scans both the request and every retrieved doc. Local-first (runs offline on Ollama qwen3:8b or any OpenAI-compatible endpoint), with an MCP server and OpenAI-compatible proxy wired into 18 coding agents (Claude Code, Codex, Cursor, Windsurf, etc.)
- **[YYLO](https://github.com/yylo-dev/yylo)**: Command-line orchestrator for coding agents that isolates each task in a dedicated branch/worktree for scoped implementation and focused tests, with typed task, validation, merge, and release-readiness boundaries
- **[ctx-optimize](https://github.com/muthuishere/ctx-optimize)**: Deterministic code knowledge graph for coding agents — one static Go binary indexes a repo, and optionally its databases, buckets, queues and APIs, into a local graph, so the agent answers "who calls this" / "what breaks if I change this" from the store in one call instead of a grep-and-read chain; ships an agent skill for Claude Code and Codex, with no LLM, embeddings, database, MCP, or credentials at rest
- **[CueParcel](https://github.com/kallist/CueParcel)**: Local-first browser extension for selecting sections of web pages, combining multiple sources, and packaging source-grounded context for AI agents, with structured TaskSpec output and GitHub Issue, GitHub Pull Request, and technical-documentation adapters
- **[Archcore](https://github.com/archcore-ai/archcore)**: Git-native context engineering and spec-driven development for AI coding agents
- **[YYLO Benchmark](https://github.com/yylo-dev/yylo-benchmark)**: Coding-agent evaluation runner for trusted hosts: runs each attempt in a fresh repository and retains outputs for independent re-evaluation; workspace separation provides context hygiene, not a security sandbox
- **[open-bridge](https://github.com/bks-lab/open-bridge)**: Plain-text git workspace that coding agents read at session start, with a CI-enforced byte budget for always-on context and on-demand indexes for everything else
- **[Curtain](https://github.com/lukstei/curtain)**: Step-by-step skill runner that reveals instructions only when their stage is reached, with explicit human approval gates between stages.
- **[Bandpass](https://github.com/MajidAsghariTabrizi/bandpass)**: A context compiler for AI agents that turns memory, state, files, and evidence into task-scoped, provenance-aware Context Capsules with token budgeting, conflict resolution, explicit unknowns, and MCP tools

### Development Frameworks

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**: Low-level orchestration framework for context management
- **[LangSmith](https://docs.smith.langchain.com/)**: Agent tracing and evaluation platform
- **[LangMem](https://langchain-ai.github.io/langmem/)**: Memory management abstractions

### Memory & Compression

- **[claude-mem](https://github.com/thedotmack/claude-mem)**: Persistent cross-session memory for Claude Code that automatically captures, compresses, and re-injects context across sessions
- **[caveman](https://github.com/JuliusBrussee/caveman)** ⭐115k: Viral skill + proxy that cuts coding-agent output tokens ~65% by making the agent talk tersely
- **[Hindsight](https://github.com/vectorize-io/hindsight)** ⭐53.2k: Agent memory that learns — consolidates experiences into reusable knowledge
- **[context-mode](https://github.com/mksglu/context-mode)** ⭐32k: Context window optimization for coding agents — sandboxes tool output (up to 98% reduction), persists session memory and enforces routing across 17 platforms via MCP + hooks
- **[TencentDB Agent Memory](https://github.com/TencentCloud/TencentDB-Agent-Memory)** ⭐29.9k: Team-level memory hub that turns conversations, docs and code into reusable memory assets (chat memory, skills, LLM-Wiki, code)
- **[Beads](https://github.com/gastownhall/beads)** ⭐28.2k: A memory upgrade for coding agents — a git-backed issue/task graph the agent reads instead of long markdown plans
- **[ai-memory](https://github.com/akitaonrails/ai-memory)** ⭐13.1k: Long-term memory for coding-agent CLIs that also enables handoff between agent vendors
- **[Engram](https://github.com/Gentleman-Programming/engram)** ⭐7.6k: Agent-agnostic persistent memory in one Go binary (SQLite + FTS5) with MCP server, HTTP API, CLI and TUI
- **[dsh-context](https://github.com/bowenliang123/dsh-context)** ⭐3.5k: DeepSeek Harness plugin for context insight — dashboard, browser and sidebar for context statistics and composition
- **[agent-beacon](https://github.com/Asymptote-Labs/agent-beacon)** ⭐2.7k: Cross-harness, self-improving memory layer for AI agents
- **[billion-context](https://github.com/ranxianglei/billion-context)** ⭐2.5k: Context-compression plugin for month-long single sessions on small (≈100K) context windows
- **[Data Olympus](https://github.com/knaisoma/data-olympus)**: Governed project memory for coding agents. Agents can propose context, humans promote accepted rules, and retrieval serves only in-force knowledge after validity-window and supersession checks
- **[GoodMemory](https://github.com/hjqcan/GoodMemory)**: Local-first, auditable memory layer for AI apps and coding agents, with Codex/Claude Code host integration, a standalone MCP server, TypeScript/Python clients, and SQLite or Postgres storage
- **[LeanCTX (lean-ctx)](https://github.com/yvgude/lean-ctx)**: Context intelligence layer for AI agents — a single local Rust binary that decides what agents read, remember, and save; 60–90% fewer tokens, with 76 MCP tools and local-first design
- **[headroom](https://github.com/chopratejas/headroom)**: Compresses tool outputs/logs before they reach the LLM, saving 60-95% of tokens
- **[Accordion](https://github.com/a-Fig/Accordion)**: A pi extension that renders an agent's entire context window as a live "map" and folds (reversibly compacts) less-relevant blocks in the background via pluggable "conductors" that rank block relevance, instead of lossy all-or-nothing compaction
- **[Mnemoverse](https://github.com/mnemoverse/mcp-memory-server)**: Hosted persistent memory reachable over MCP from Claude Code, Cursor, VS Code and ChatGPT with one key or OAuth; recall is re-ranked by whether a recalled memory helped or misled
- **[Letta (MemGPT)](https://github.com/letta-ai/letta)**: Framework for building stateful agents with long-term memory
- **[Mem0](https://github.com/mem0ai/mem0)**: Memory layer for AI agents and assistants
- **[Vestige](https://github.com/samvallad33/vestige)**: "Local-first MCP memory server (single ~23MB Rust binary, data stays on-device) with entity/causal retrieval — links memories by shared file/env-var/service/symbol to recall the earlier change behind a later failure that similarity search can miss"
- **[Tree Ring Memory](https://github.com/TerminallyLazy/Tree-Ring-Memory)**: Lifecycle-aware local-first memory framework for AI agents with recall, audit, forgetting, consolidation, and project-scoped `.tree-ring` storage
- **[LLMLingua](https://github.com/microsoft/LLMLingua)**: Prompt compression for accelerated and cost-efficient LLM inference
- **[LLMSlim](https://github.com/Thanatos9404/llmslim)**: Open-source (MIT) Python context compression for RAG and agents. It selects query-relevant units under a hard token budget, lexically or with an optional 22M cross-encoder, and publishes a reproducible head-to-head against LLMLingua-2 and LongLLMLingua
- **[Cognee](https://github.com/topoteretes/cognee)**: Open-source AI memory platform giving agents persistent long-term memory via a self-hosted knowledge-graph engine
- **[Graphiti](https://github.com/getzep/graphiti)**: Framework for building real-time, temporal knowledge graphs for AI agents (the engine behind Zep's memory)
- **[Supermemory](https://github.com/supermemoryai/supermemory)**: Fast, scalable memory + context engine with a single Memory API; supports fully local runs
- **[skillreaper](https://github.com/thousandflowers/skillreaper)**: Measures what share of an agent's loaded context ever actually fires — skills, MCP servers, subagents, hooks — from session transcripts, then prunes the unused parts reversibly
- **[LWC](https://github.com/JanYork/llm-wiki-cli)**: Proactive, source-grounded project memory for coding agents, preserving immutable sources, citations, provenance, and atomic changesets with SQLite/FTS5 retrieval, optional document/code graphs, lifecycle hooks, and a bounded one-tool MCP interface
- **[billion-context-dsh](https://github.com/Tyan66666/billion-context-dsh)**: Model-driven context compression (Active Context Pruning) for the DeepSeek Harness — the model decides when and what to compress
- **[TekMyra](https://github.com/laconiq-ai/tekmyra)**: Governed context compression for LLM pipelines that checks every protected span (identifiers, amounts, citations) survives before it emits, refuses when it cannot, and counts refusals against its published numbers
- **[trace-mcp](https://github.com/nikolai-vysotskyi/trace-mcp)**: Local MCP server that serves a precomputed cross-language code graph instead of file reads — framework-aware edges link a PHP controller to the Vue page it renders, a DI decorator to its provider, an ORM call to the migration that defines the table (81 languages, 87 framework integrations), plus code-linked decision memory across sessions; on 60 merged PRs from six third-party repositories, review context drops from a median 13,595 to 1,326 input tokens
- **[langstate](https://github.com/hermes-labs-ai/langstate)**: Compresses long LLM conversations into an inspectable `[SCAFFOLD STATE]` message instead of an opaque summary — keeps system messages and recent turns verbatim, and `validate()` returns a deterministic receipt for named facts
- **[Caura](https://github.com/caura-ai/caura)**: Governed shared memory for AI agent fleets — scoped recall across agents and tenants, trust tiers, keystone policies, audit trails, and a knowledge graph, exposed over MCP
- **[Statewave](https://github.com/smaramwbc/statewave)**: Memory runtime that compiles agent events into structured memories and assembles reproducible, provenance-tagged context bundles per subject instead of query-time retrieval, self-hosted on Postgres + pgvector with Python/TypeScript SDKs and an MCP server
- **[Ctxlens](https://github.com/AgentPostmortem/Ctxlens)**: Context-window profiler for AI agents: shows what is eating your tokens
- **[tokencut](https://github.com/AgentPostmortem/tokencut)**: Measures and cuts the token cost of LLM and agent message payloads
- **[contextpress](https://github.com/Taha-azizi/contextpress)**: Deterministic context compression for LLM chat, RAG, and agent pipelines (`pip install contextpress`) — Tier-1 NLP stages, no API key; optional Tier-2 LLM backends
- **[toldya](https://github.com/singhlabsdev/toldya)**: Turns the corrections you keep repeating to Claude Code (found in local session history) into CLAUDE.md / AGENTS.md rules after per-rule approval, then counts whether each rule stopped the repeats
- **[Hyperconsciousness](https://github.com/louis030195/hyperconsciousness)**: Developer-alpha Rust knowledge store with encrypted, append-only records, device sync, and MCP retrieval constrained by scoped, expiring grants
- **[elephant](https://github.com/tonone-ai/elephant)**: Claude Code plugin for persistent per-repo memory — keeps an `ELEPHANT.md` in the repo root that is auto-loaded at session start and committed so teammates share the same context, plus a global cross-repo memory file, cold-start seeding from git history, and compaction of old entries
- **[context-bank](https://github.com/kadiresen/context-bank)**: Git-committed, size-capped project memory for coding agents under .ai/, with retrieval-first history, a doctor check and Cline Memory Bank migration
- **[Gobstopper](https://github.com/hraness/gobstopper)**: Local proxy for Claude Code, Codex, and other coding agents that replaces older turns with a mechanical summary once a request passes a token threshold and sends the newest turns verbatim; can also prepare smaller copies of saved sessions
- **[Agent-Body](https://github.com/1420079678-ctrl/agent-body)**: Plugin layer for DeepSeek Harness with intent-based tool-schema gating, deterministic reflexes, and idle-time memory consolidation; includes an offline demo and a reproducible tool-schema token benchmark
- **[Context Guru](https://github.com/rossoctl/context-guru)**: Provider-agnostic context engine combining prompt-cache management, tool-output filtering, and context compaction, with Claude Code and Codex integrations
- **[Dory](https://github.com/tjqscott/dory)**: Project memory in a linked Markdown wiki, with decisions, findings and a taskboard; Claude Code hooks load the index at session start and check wiki structure after changes
- **[Sunstone](https://github.com/space-bacon/sunstone-vscode)**: Source-available (BSL 1.1) VS Code extension that indexes the folders you open, on your machine, and gives any model in the picker, Copilot's included, a tool to search them; for your own model servers, the turns and tool results a window cannot hold are folded into that local store and come back when they are the closest match to the question, with a recall tool to search them
- **[ctxprune](https://github.com/riceharvest/ctxprune)**: Small token-classifier model (LLMLingua-2 style, CPU/ONNX) that deletes low-value words from tool outputs, logs and documents while keeping IDs, paths and numbers intact (`pip install ctxprune`)
- **[mu](https://github.com/qybaihe/mu)**: Coding agent built on pi that hands context decisions to a small judge model: long tool output is judged chunk by chunk and only what matters now enters the context (the rest is archived behind a pointer), stale tool results become one-line tombstones above a context threshold, compaction keeps or prunes passages without writing a summary, and exact repeats in failing test logs can be folded losslessly (51% fewer characters on the authors' seven largest real logs)
- **[akc-cycle](https://github.com/shimo4228/akc-cycle)**: Claude Code plugin that keeps the rules and skills in a setup from piling up and going stale; it audits skills, rules and agent definitions, proposes keep, merge or retire for each file with a reason, changes nothing until you confirm, and promotes advice that keeps coming back into always-loaded rules
- **[contexttax](https://github.com/henrikdukefoss/contexttax)**: Measures the fixed context a coding agent loads on every request (CLAUDE.md/AGENTS.md and their `@` imports, always-on rules, skill descriptions, MCP tool schemas) per agent, largest first, and points out what can load on demand instead
- **[turntax](https://github.com/TorutDev/turntax)**: Measures the per-turn token cost of agent instruction files (AGENTS.md, CLAUDE.md, Cursor, Copilot and Windsurf rules) for 16 coding agents from each agent's documented loading rules, separates always-loaded from on-demand files, and previews safe fixes; CLI, GitHub Action and read-only MCP server
- **[gknowledge](https://github.com/openapi/gknowledge)**: Local-first Karpathy LLM Wiki as a native GNOME app: sources stay immutable while each ingest updates only the Markdown pages they touch (provenance in YAML frontmatter, `index.md` catalogue, append-only `log.md`, cross-source name merging), so questions are answered from the compiled wiki with citations instead of re-reading raw documents; an MCP server (`search_wiki`, `read_page`, `ask`, `add_source`) lets Claude Code, Codex or Cursor read and grow the same wiki, and an offline engine works with no model or API key (MIT)
- **[Chisle](https://github.com/JayPokale/Chisle)**: Plugin for Claude Code, Pi, OpenCode and Copilot CLI (rule files for nine more agents) that shrinks oversized tool output before it re-enters the context (head and tail kept, error lines salvaged, same-session dedup; Read/Edit/Write output never touched) and adds context-diet rules (grep before reading, sliced reads, no re-reads); tool output was 67.5% of the context in 171 measured Claude Code sessions, and the hook trims eligible outputs by about 46%. Deterministic, no dependencies or network calls
- **[Next Context](https://github.com/ajwann/next-context)**: Claude Code plugin that keeps a live context-window band above the prompt, showing usage by `/context` category, the overhead loaded before the first message with the largest MCP server, growth per turn, and a projection of how many turns remain before auto-compaction, with user-set warning thresholds; runs locally with no network or API calls
- **[Off the Wall](https://github.com/toranalytics/off-the-wall)**: Claude Code and Codex plugin that keeps the messages in which you corrected the agent and quotes the most relevant ones back, word for word, before it answers, instead of summarising them into rules; `otw report` shows what you correct most, and a routing rule asks the orchestrator to name a model for each subagent call. No API key, no dependencies
- **[agent-files-check](https://github.com/lesshands/agent-files-check)**: Checks coding-agent instruction files (AGENTS.md, CLAUDE.md, GEMINI.md, Cursor, Copilot, Windsurf and Cline rules) for references to files and folders that git history shows were deleted or moved, with file:line, the commit and the new location when it can tell; as a GitHub Action it notes on a pull request each instruction line whose path the change deletes or renames
- **[GFT Map](https://github.com/CoralLips/gft-map)**: A local app for Claude Code and Codex that stores decisions, reasons, and open questions in editable documents and maps for topic-based context readback
- **[lean-agent-tools](https://github.com/SirGrokkius/lean-agent-tools)**: 14 single-file, standard-library-only Python CLI filters that sit between shell output and the model: `logdiff` collapses logs into counted templates (errors kept), `jsonslim` projects JSON to named paths, `htmltext` returns readable page text under a budget, plus caching, change-only polling and a dependency-free token estimator; published per-tool before/after token counts (o200k)
- **[lcc](https://github.com/lucasmartins-ai/lcc)**: Compacts coding-agent sessions without a summary: spent tool outputs move to a local content-addressed stash the agent can `recall` by hash, user and assistant messages stay byte for byte, and every pass writes a JSON report of what was cut and why; offline, as a CLI, an MCP server and a Claude Code plugin

### Production Tools

- **Claude Code**: Auto-compact context management
- **ChatGPT**: Long-term cross-session memory
- **[Puppyone](https://github.com/puppyone-ai/puppyone)**: File-system-based context storage for agents
- **Cursor**: Rules-based context engineering
- **Windsurf**: Advanced code context retrieval
- **[OrcaReplay](https://github.com/Continuum-AI-Corp/OrcaReplay)**: Records the context an agent actually sent, turn by turn, and replays it offline
- **[Praxix](https://praxix.app)**: Commercial macOS app for Claude Code that curates per-session context: pick exactly which skills, rules, and references each session loads (presets, per-project defaults, a lean CLAUDE.md), and see that context before a token is spent
- **[Aident Loadout](https://aident.ai/)**: Hosted MCP server, CLI and Skill for agent tool context: agents search actions by intent and fetch the live schema only for the action they run, can start from 400+ expert-built Skills, and never see raw API keys (credentials stay in Aident Vault, outside the model context)

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
6. **Progressive Disclosure**: Keep tool and skill definitions out of static context until needed; load full schemas/instructions on demand
7. **Measure the Harness**: Track cache hit rate, per-turn fixed context and token cost per task; the same model can swing 2× across harnesses
8. **Treat Summaries as Untrusted**: Compaction is lossy and can carry injected instructions — keep raw history recoverable and audit what was dropped

## 🔗 Model Context Protocol (MCP)

### Context7 MCP Server
**Up-to-date code documentation for LLMs and AI code editors**
- 🔗 Repository: [upstash/context7](https://github.com/upstash/context7)
- 🎯 Real-time code context for development workflows
- 🚀 Integration with popular AI coding assistants

### MCP Ecosystem
- **[Model Context Protocol](https://modelcontextprotocol.io/introduction)**: Standardized context sharing
- **[Filesystem Server](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)**: File-based context management
- **[installed-api](https://github.com/ajwann/installed-api)**: MCP server (packaged as a Claude Code plugin) that answers Python and TypeScript API questions from the packages installed in the project rather than from model memory or web docs: signatures, docstrings, source and versions from the project's own interpreter, and `.d.ts` declarations from `node_modules`, returned as bounded excerpts instead of whole files; Python standard library only, no network requests
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

This project is licensed under the [CC0 1.0](LICENSE).

## 🙏 Acknowledgments

Special thanks to all contributors and the research community advancing the field of context engineering.

---

**Maintained by**: [yzfly](https://github.com/yzfly) | **云中江树（微信公众号: 云中江树）**

*If you find this repository helpful, please consider giving it a ⭐!*
