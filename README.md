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


### LangChain Context Engineering
**Context Engineering for Agents**
- 📄 Original: [LangChain Blog](https://blog.langchain.com/context-engineering-for-agents/)
- 🇨🇳 Chinese Translation: [智能体的Context工程-中文版](docs/langchain/智能体的Context工程-中文版.md)

Comprehensive guide covering four key strategies:
- **Write Context**: Saving information outside context window
- **Select Context**: Pulling relevant information into context
- **Compress Context**: Retaining only necessary tokens
- **Isolate Context**: Splitting context across different spaces

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

**Context Engineering 2.0: The Context of Context Engineering**
- 📄 arXiv: [2510.26493](https://arxiv.org/abs/2510.26493)
- 🎯 Traces context engineering back two decades and proposes a systematic definition and design principles across the human-computer to human-agent evolution

**Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models**
- 📄 arXiv: [2510.04618](https://arxiv.org/abs/2510.04618)
- 🎯 Treats context as an evolving playbook (Generator/Reflector/Curator) to prevent context collapse; reports +10.6% on agents and +8.6% on finance tasks (ICLR 2026)

**Memory in the Age of AI Agents: A Survey**
- 📄 arXiv: [2512.13564](https://arxiv.org/abs/2512.13564)
- 🎯 A "forms-functions-dynamics" taxonomy of agent memory, distinguishing it from RAG and context engineering

**PerspectiveGap: A Benchmark for Multi-Agent Orchestration Prompting**
- 📄 arXiv: [2606.08878](https://arxiv.org/abs/2606.08878)
- 📊 110 scenarios across 10 topologies, testing how LLMs allocate context fragments to sub-agent roles
- 🎯 The first benchmark for multi-agent orchestration prompt writing; 33 commercial models average 17.2% pass rate on role-fragment assignment

> *Evaluates LLMs' ability to assign information fragments to sub-agent roles in multi-agent orchestration scenarios. Scoring uses strict pass (zero omissions and zero leaks) plus partial-credit metrics. Data covers 10 loop-centered topologies across 100 professional domains.*

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
- **[Agent Skills open standard](https://agentskills.io/)**: `SKILL.md` folders loaded via progressive disclosure (name/description → full instructions → bundled files), adopted by 40+ agents (Claude Code, Codex, Gemini CLI, Cursor, OpenCode, dsh...). Skills are the portable unit of *procedural* context
- **[MCP 2026-07-28 spec](https://blog.modelcontextprotocol.io/posts/2026-07-28/)**: stateless core, cacheable `tools/list` results with `ttlMs`, and header-based routing — all aimed at keeping tool context cheap and fresh for agents
- **[Claude Code context docs](https://code.claude.com/docs/en/memory)**: CLAUDE.md memory hierarchy, `/compact`, subagents and hooks — the most widely copied practical context-management model

### Context Engineering Systems & Kits

- **[Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)** ⭐76.1k: Comprehensive guide and resources for prompt engineering, context engineering, RAG, and AI agents
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)** ⭐64.5k: Meta-prompting, context engineering and spec-driven development system for Claude Code
- **[screenpipe](https://github.com/screenpipe/screenpipe)** ⭐19.9k: Source-available, local-first work-context layer that captures screen, audio, input, browser, and meeting activity for agent retrieval through MCP, REST, and CLI
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)** ⭐16.7k: Collection of Agent Skills for context engineering and multi-agent architectures
- **[Context Engineering Intro](https://github.com/coleam00/context-engineering-intro)** ⭐13.5k: Context engineering introduction and methodology for AI coding assistants
- **[Context-Engineering (jasontang-ai)](https://github.com/jasontang-ai/Context-Engineering)** ⭐9.1k: Karpathy-inspired first-principles handbook for context engineering
- **[GSD-2](https://github.com/gsd-build/gsd-2)** ⭐7.7k: Meta-prompting / context engineering system enabling long-running autonomous agent work
- **[MineContext](https://github.com/volcengine/MineContext)** ⭐5.4k: Volcengine's proactive, context-aware AI companion
- **[Awesome-Context-Engineering (Meirtz)](https://github.com/Meirtz/Awesome-Context-Engineering)** ⭐3.2k: Comprehensive survey of context engineering
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)** ⭐2.7k: Deep dive into Claude Code source: architecture, agent loop, and context engineering
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)** ⭐2.3k: Self-correcting memory Claude Code workflow, including context engineering
- **[Advanced Context Engineering for Coding Agents](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)** ⭐1.7k: Advanced context engineering techniques for coding agents
- **[Context Engineering Kit](https://github.com/NeoLabHQ/context-engineering-kit)** ⭐1.2k: A context engineering skills toolkit
- **[ACE (Agentic Context Engineering)](https://github.com/ace-agent/ace)** ⭐1.2k: Evolving language agents through agentic context engineering (ACE)
- **[context-space](https://github.com/context-space/context-space)** ⭐812: Context engineering infrastructure built from MCPs and integrations
- **[Agent QA](https://github.com/vostride/agent-qa)** ⭐772: Source-available QA agent that retains scoped test memory and failure evidence across natural-language web and mobile runs
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)** ⭐708: A practical, hands-on guide (in Chinese) to context engineering for LLM applications
- **[AI Badger](https://github.com/PVRLabs/aibadger)** ⭐21: Local-first tool that extracts focused, token-efficient repository context for AI chats, with Agent Skills for code review and coding-agent handoff
- **[get-shit-done (GSD)](https://github.com/gsd-build/get-shit-done)**: Meta-prompting and spec-driven development / context engineering system for Claude Code
- **[GSD-2](https://github.com/gsd-build/gsd-2)**: Meta-prompting / context engineering system enabling long-running autonomous agent work
- **[how-claude-code-works](https://github.com/Windy3f3f3f3f/how-claude-code-works)**: Deep dive into Claude Code source: architecture, agent loop, and context engineering
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)**: Collection of Agent Skills for context engineering and multi-agent architectures
- **[Advanced Context Engineering for Coding Agents](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents)**: Advanced context engineering techniques for coding agents
- **[ax](https://github.com/Necmttn/ax)**: Local telemetry and recall graph for AI coding-agent sessions, costs, skills, routing, and hooks
- **[ACE (Agentic Context Engineering)](https://github.com/ace-agent/ace)**: Evolving language agents through agentic context engineering
- **[Context Engineering Kit](https://github.com/NeoLabHQ/context-engineering-kit)**: A toolkit for context engineering
- **[MineContext](https://github.com/volcengine/MineContext)**: Volcengine's proactive, context-aware AI companion
- **[pro-workflow](https://github.com/rohitg00/pro-workflow)**: Self-correcting memory system that lets Claude Code learn from its mistakes, covering context engineering, parallel worktrees, agent teams, and 17 skills
- **[kit](https://github.com/cased/kit)**: Context engineering toolkit for AI dev tools, providing codebase mapping, symbol extraction, and code search
- **[RepoPrompt CE](https://github.com/repoprompt/repoprompt-ce)**: Community edition of RepoPrompt, a native macOS context engineering app for AI coding agents, with an MCP CLI
- **[DevProjex](https://github.com/Avazbek22/DevProjex)**: Builds clean, token-aware project context with GUI, TUI, CLI, and a read-only MCP server, plus live preview, secret redaction enforced in MCP mode, Git scopes, and syntax-aware compression.
- **[context-engineering (outcomeops)](https://github.com/outcomeops/context-engineering)**: Working reference implementation of context engineering with five components (corpus, retrieval, injection, output, enforcement), running end-to-end on Amazon Bedrock
- **[Project Context Records (PCR)](https://github.com/hyf0/project-context-records)**: A context-engineering methodology that keeps a durable, repo-versioned archive of a project's meta-context (the why, architecture, and maintainer decisions) so AI collaborators inherit its judgment instead of re-deriving it
- **[interview-prep-template](https://github.com/AbhiK189/interview-prep-template)**: A three-layer context-engineering template (immutable sources → agent-maintained wiki → operating-manual file) where the agent synthesizes raw material into reusable answers, frameworks, and scored debriefs that compound over time
- **[Practical Guide to Context Engineering](https://github.com/WakeUp-Jin/Practical-Guide-to-Context-Engineering)**: A practical, hands-on guide (in Chinese) to context engineering for LLM applications
- **[MFS](https://github.com/zilliztech/mfs)**: A context harness that unifies code, docs, chat, databases, and object stores into one file-like, searchable namespace (`ls`/`cat`/`grep` + hybrid semantic search), so agents pull context incrementally instead of injecting it all up front; self-hosted and local-first (local ONNX embeddings + Milvus, no API key)
- **[wiki](https://github.com/plasma-ai/wiki)**: Indexed Markdown knowledge bases that let agents retrieve project context incrementally, with deterministic indexes, cross-links, linting, and merge handling for parallel edits
- **[Hexis](https://github.com/Bevel-Software/Hexis)**: Git-backed platform for skills, tools, and context for AI agents, with Git review workflows, role-based access, encrypted secrets, and remote MCP access
- **[PickySteve](https://github.com/KernelLord/pickysteve)**: A skill router and context picker for coding agents — hybrid BM25 + embedding retrieval (RRF-fused), cross-encoder rerank with a calibrated floor, and an LLM judge select the right skill, then assemble a minimal nonce-boundaried context bundle for the execution model; a fail-closed ONNX prompt-injection gate scans both the request and every retrieved doc. Local-first (runs offline on Ollama qwen3:8b or any OpenAI-compatible endpoint), with an MCP server and OpenAI-compatible proxy wired into 18 coding agents (Claude Code, Codex, Cursor, Windsurf, etc.)
- **[YYLO](https://github.com/yylo-dev/yylo)**: Command-line orchestrator for coding agents that isolates each task in a dedicated branch/worktree for scoped implementation and focused tests, with typed task, validation, merge, and release-readiness boundaries
- **[ctx-optimize](https://github.com/muthuishere/ctx-optimize)**: Deterministic code knowledge graph for coding agents — one static Go binary indexes a repo, and optionally its databases, buckets, queues and APIs, into a local graph, so the agent answers "who calls this" / "what breaks if I change this" from the store in one call instead of a grep-and-read chain; ships an agent skill for Claude Code and Codex, with no LLM, embeddings, database, MCP, or credentials at rest
- **[CueParcel](https://github.com/kallist/CueParcel)**: Local-first browser extension for selecting sections of web pages, combining multiple sources, and packaging source-grounded context for AI agents, with structured TaskSpec output and GitHub Issue, GitHub Pull Request, and technical-documentation adapters

### Development Frameworks

- **[LangGraph](https://langchain-ai.github.io/langgraph/)**: Low-level orchestration framework for context management
- **[LangSmith](https://docs.smith.langchain.com/)**: Agent tracing and evaluation platform
- **[LangMem](https://langchain-ai.github.io/langmem/)**: Memory management abstractions

### Memory & Compression

- **[claude-mem](https://github.com/thedotmack/claude-mem)**: Persistent cross-session memory for Claude Code that automatically captures, compresses, and re-injects context across sessions
- **[Data Olympus](https://github.com/knaisoma/data-olympus)**: Governed project memory for coding agents. Agents can propose context, humans promote accepted rules, and retrieval serves only in-force knowledge after validity-window and supersession checks
- **[GoodMemory](https://github.com/hjqcan/GoodMemory)**: Local-first, auditable memory layer for AI apps and coding agents, with Codex/Claude Code host integration, a standalone MCP server, TypeScript/Python clients, and SQLite or Postgres storage
- **[LeanCTX (lean-ctx)](https://github.com/yvgude/lean-ctx)**: Context intelligence layer for AI agents — a single local Rust binary that decides what agents read, remember, and save; 60–90% fewer tokens, with 76 MCP tools and local-first design
- **[headroom](https://github.com/chopratejas/headroom)**: Compresses tool outputs/logs before they reach the LLM, saving 60-95% of tokens
- **[Accordion](https://github.com/a-Fig/Accordion)**: A pi extension that renders an agent's entire context window as a live "map" and folds (reversibly compacts) less-relevant blocks in the background via pluggable "conductors" that rank block relevance, instead of lossy all-or-nothing compaction
- **[Mnemoverse](https://github.com/mnemoverse/mcp-memory-server)**: Hosted persistent memory reachable over MCP from Claude Code, Cursor, VS Code and ChatGPT with one key; recall is re-ranked by whether a recalled memory helped or misled
- **[Letta (MemGPT)](https://github.com/letta-ai/letta)**: Framework for building stateful agents with long-term memory
- **[Mem0](https://github.com/mem0ai/mem0)**: Memory layer for AI agents and assistants
- **[Vestige](https://github.com/samvallad33/vestige)**: "Local-first MCP memory server (single ~23MB Rust binary, data stays on-device) with entity/causal retrieval — links memories by shared file/env-var/service/symbol to recall the earlier change behind a later failure that similarity search can miss"
- **[Tree Ring Memory](https://github.com/TerminallyLazy/Tree-Ring-Memory)**: Lifecycle-aware local-first memory framework for AI agents with recall, audit, forgetting, consolidation, and project-scoped `.tree-ring` storage
- **[LLMLingua](https://github.com/microsoft/LLMLingua)**: Prompt compression for accelerated and cost-efficient LLM inference
- **[Cognee](https://github.com/topoteretes/cognee)**: Open-source AI memory platform giving agents persistent long-term memory via a self-hosted knowledge-graph engine
- **[Graphiti](https://github.com/getzep/graphiti)**: Framework for building real-time, temporal knowledge graphs for AI agents (the engine behind Zep's memory)
- **[Supermemory](https://github.com/supermemoryai/supermemory)**: Fast, scalable memory + context engine with a single Memory API; supports fully local runs
- **[skillreaper](https://github.com/thousandflowers/skillreaper)**: Measures what share of an agent's loaded context ever actually fires — skills, MCP servers, subagents, hooks — from session transcripts, then prunes the unused parts reversibly
- **[LWC](https://github.com/JanYork/llm-wiki-cli)**: Proactive, source-grounded project memory for coding agents, preserving immutable sources, citations, provenance, and atomic changesets with SQLite/FTS5 retrieval, optional document/code graphs, lifecycle hooks, and a bounded one-tool MCP interface
- **[billion-context-dsh](https://github.com/Tyan66666/billion-context-dsh)**: Model-driven context compression (Active Context Pruning) for the DeepSeek Harness — the model decides when and what to compress
- **[TekMyra](https://github.com/laconiq-ai/tekmyra)**: Governed context compression for LLM pipelines that verifies protected spans are present exactly once before emitting and counts refusals against its published numbers
- **[trace-mcp](https://github.com/nikolai-vysotskyi/trace-mcp)**: Local MCP server that serves a precomputed cross-language code graph instead of file reads — framework-aware edges link a PHP controller to the Vue page it renders, a DI decorator to its provider, an ORM call to the migration that defines the table (81 languages, 87 framework integrations), plus code-linked decision memory across sessions; on 60 merged PRs from six third-party repositories, review context drops from a median 13,595 to 1,326 input tokens
- **[langstate](https://github.com/hermes-labs-ai/langstate)**: Compresses long LLM conversations into an inspectable `[SCAFFOLD STATE]` message instead of an opaque summary — keeps system messages and recent turns verbatim, and `validate()` returns a deterministic receipt for named facts
- **[Caura](https://github.com/caura-ai/caura)**: Governed shared memory for AI agent fleets — scoped recall across agents and tenants, trust tiers, keystone policies, audit trails, and a knowledge graph, exposed over MCP
- **[Ctxlens](https://github.com/AgentPostmortem/Ctxlens)**: Context-window profiler for AI agents: shows what is eating your tokens
- **[tokencut](https://github.com/AgentPostmortem/tokencut)**: Measures and cuts the token cost of LLM and agent message payloads
- **[contextpress](https://github.com/Taha-azizi/contextpress)**: Deterministic context compression for LLM chat, RAG, and agent pipelines (`pip install contextpress`) — Tier-1 NLP stages, no API key; optional Tier-2 LLM backends
- **[contextdiet](https://github.com/devilking7x/contextdiet)**: Local-first context-window analyzer for AI coding agents — real BPE token counts for CLAUDE.md, skill files, and `.mcp.json`, per-file breakdown ranked by size, %-of-window gauge with the 20% rule, impact-ordered trim suggestions, and a before/after savings simulator. Live demo included.

### Production Tools

- **Claude Code**: Auto-compact context management
- **ChatGPT**: Long-term cross-session memory
- **[Puppyone](https://github.com/puppyone-ai/puppyone)**: File-system-based context storage for agents
- **Cursor**: Rules-based context engineering
- **Windsurf**: Advanced code context retrieval
- **[OrcaReplay](https://github.com/Continuum-AI-Corp/OrcaReplay)**: Records the context an agent actually sent, turn by turn, and replays it offline

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

This project is licensed under the [CC0 1.0](LICENSE).

## 🙏 Acknowledgments

Special thanks to all contributors and the research community advancing the field of context engineering.

---

**Maintained by**: [yzfly](https://github.com/yzfly) | **云中江树（微信公众号: 云中江树）**

*If you find this repository helpful, please consider giving it a ⭐!*
