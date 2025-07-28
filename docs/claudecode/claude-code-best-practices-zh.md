标题：Claude Code 最佳实践

原文链接：https://www.anthropic.com/engineering/claude-code-best-practices

Markdown 内容：
[Anthropic 工程团队](https://www.anthropic.com/engineering)

我们最近[发布了 Claude Code](https://www.anthropic.com/news/claude-3-7-sonnet)，这是一个用于智能编程的命令行工具。作为一个研究项目开发，Claude Code 为 Anthropic 的工程师和研究人员提供了一种更原生的方式来将 Claude 集成到他们的编程工作流程中。

Claude Code 故意设计得低层次且不带偏见，提供接近原始模型访问的能力，而不强制特定的工作流程。这种设计理念创造了一个灵活、可定制、可脚本化且安全的强大工具。虽然功能强大，但这种灵活性对于刚接触智能编程工具的工程师来说存在学习曲线——至少在他们形成自己的最佳实践之前是这样。

本文概述了已被证明有效的通用模式，这些模式适用于 Anthropic 内部团队以及在各种代码库、语言和环境中使用 Claude Code 的外部工程师。这个列表中的内容都不是一成不变的，也不是普遍适用的；请将这些建议视为起点。我们鼓励您进行实验，找到最适合您的方法！

_想要更详细的信息？我们在 [claude.ai/code](https://claude.ai/redirect/website.v1.26d5e13f-bc1f-432b-b15a-bdc59c1b87b0/code) 的综合文档涵盖了本文提到的所有功能，并提供了额外的示例、实现细节和高级技术。_

1. 自定义您的设置
-----------------------

Claude Code 是一个智能编程助手，会自动将上下文拉入提示中。这种上下文收集会消耗时间和令牌，但您可以通过环境调优来优化它。

### a. 创建 `CLAUDE.md` 文件

`CLAUDE.md` 是一个特殊文件，Claude 在开始对话时会自动将其拉入上下文。这使其成为记录以下内容的理想位置：

*   常用 bash 命令
*   核心文件和实用函数
*   代码风格指南
*   测试说明
*   仓库礼仪（例如，分支命名、合并 vs 变基等）
*   开发环境设置（例如，pyenv use，哪些编译器有效）
*   项目特有的任何意外行为或警告
*   您希望 Claude 记住的其他信息

`CLAUDE.md` 文件没有必需的格式。我们建议保持简洁和人类可读。例如：

```
# Bash 命令
- npm run build: 构建项目
- npm run typecheck: 运行类型检查器

# 代码风格
- 使用 ES 模块（import/export）语法，而不是 CommonJS（require）
- 尽可能解构导入（例如 import { foo } from 'bar'）

# 工作流程
- 完成一系列代码更改后，请确保进行类型检查
- 为了性能考虑，优先运行单个测试，而不是整个测试套件
```

您可以将 `CLAUDE.md` 文件放在几个位置：

*   **仓库的根目录**，或您运行 `claude` 的任何地方（最常见的用法）。将其命名为 `CLAUDE.md` 并检入 git，这样您可以在会话之间和团队中共享它（推荐），或将其命名为 `CLAUDE.local.md` 并将其添加到 `.gitignore`
*   **运行 `claude` 的目录的任何父目录**。这对于单体仓库最有用，您可能从 `root/foo` 运行 `claude`，并在 `root/CLAUDE.md` 和 `root/foo/CLAUDE.md` 中都有 `CLAUDE.md` 文件。这两个都会自动拉入上下文
*   **运行 `claude` 的目录的任何子目录**。这与上面相反，在这种情况下，当您在子目录中处理文件时，Claude 会按需拉入 `CLAUDE.md` 文件
*   **您的主文件夹**（`~/.claude/CLAUDE.md`），这会将其应用到您的所有 _claude_ 会话

当您运行 `/init` 命令时，Claude 会自动为您生成一个 `CLAUDE.md`。

### b. 调优您的 `CLAUDE.md` 文件

您的 `CLAUDE.md` 文件会成为 Claude 提示的一部分，因此应该像任何经常使用的提示一样进行优化。一个常见的错误是添加大量内容而不迭代其有效性。花时间实验并确定什么能从模型中产生最佳的指令遵循效果。

您可以手动向 `CLAUDE.md` 添加内容，或按 `#` 键给 Claude 一个指令，它会自动将其合并到相关的 `CLAUDE.md` 中。许多工程师在编程时频繁使用 `#` 来记录命令、文件和风格指南，然后在提交中包含 `CLAUDE.md` 更改，以便团队成员也能受益。

在 Anthropic，我们偶尔通过[提示改进器](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-improver)运行 `CLAUDE.md` 文件，并经常调整指令（例如添加"重要"或"您必须"的强调）以提高遵循度。

![图片 1：Claude Code 工具允许列表](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F6961243cc6409e41ba93895faded4f4bc1772366-1600x1231.png&w=3840&q=75)

### c. 管理 Claude 的允许工具列表

默认情况下，Claude Code 会为任何可能修改您系统的操作请求权限：文件写入、许多 bash 命令、MCP 工具等。我们故意采用这种保守的方法来设计 Claude Code，以优先考虑安全性。您可以自定义允许列表以允许您知道安全的其他工具，或允许容易撤销的潜在不安全工具（例如，文件编辑、`git commit`）。

有四种方式管理允许的工具：

*   **在会话期间出现提示时选择"始终允许"**。
*   **启动 Claude Code 后使用 `/permissions` 命令**来添加或删除允许列表中的工具。例如，您可以添加 `Edit` 以始终允许文件编辑，`Bash(git commit:*)` 以允许 git 提交，或 `mcp__puppeteer__puppeteer_navigate` 以允许使用 Puppeteer MCP 服务器进行导航。
*   **手动编辑**您的 `.claude/settings.json` 或 `~/.claude.json`（我们建议将前者检入源代码控制以与团队共享）。
*   **使用 `--allowedTools` CLI 标志**进行会话特定的权限设置。

### d. 如果使用 GitHub，请安装 gh CLI

Claude 知道如何使用 `gh` CLI 与 GitHub 交互，用于创建问题、打开拉取请求、阅读评论等。没有安装 `gh` 的情况下，Claude 仍然可以使用 GitHub API 或 MCP 服务器（如果您已安装）。

2. 为 Claude 提供更多工具
-------------------------

Claude 可以访问您的 shell 环境，您可以在其中为它构建便利脚本和函数集合，就像您为自己做的那样。它还可以通过 MCP 和 REST API 利用更复杂的工具。

### a. 将 Claude 与 bash 工具一起使用

Claude Code 继承您的 bash 环境，使其能够访问您的所有工具。虽然 Claude 了解常见的实用程序，如 unix 工具和 `gh`，但没有说明它不会了解您的自定义 bash 工具：

1.   告诉 Claude 工具名称和使用示例
2.   告诉 Claude 运行 `--help` 查看工具文档
3.   在 `CLAUDE.md` 中记录经常使用的工具

### b. 将 Claude 与 MCP 一起使用

Claude Code 既作为 MCP 服务器又作为客户端运行。作为客户端，它可以连接到任意数量的 MCP 服务器，通过三种方式访问它们的工具：

*   **在项目配置中**（在该目录中运行 Claude Code 时可用）
*   **在全局配置中**（在所有项目中可用）
*   **在检入的 `.mcp.json` 文件中**（对在您的代码库中工作的任何人都可用）。例如，您可以将 Puppeteer 和 Sentry 服务器添加到您的 `.mcp.json`，这样在您的仓库中工作的每个工程师都可以开箱即用地使用这些。

使用 MCP 时，使用 `--mcp-debug` 标志启动 Claude 也很有帮助，以帮助识别配置问题。

### c. 使用自定义斜杠命令

对于重复的工作流程——调试循环、日志分析等——将提示模板存储在 `.claude/commands` 文件夹内的 Markdown 文件中。当您输入 `/` 时，这些会通过斜杠命令菜单变得可用。您可以将这些命令检入 git，使其对团队的其他成员可用。

自定义斜杠命令可以包含特殊关键字 `$ARGUMENTS` 来从命令调用传递参数。

例如，这里是一个您可以用来自动拉取和修复 Github 问题的斜杠命令：

```
请分析并修复 GitHub 问题：$ARGUMENTS。

按照以下步骤：

1. 使用 `gh issue view` 获取问题详情
2. 理解问题中描述的问题
3. 搜索代码库中的相关文件
4. 实施必要的更改来修复问题
5. 编写并运行测试来验证修复
6. 确保代码通过 linting 和类型检查
7. 创建描述性的提交消息
8. 推送并创建 PR

记住对所有 GitHub 相关任务使用 GitHub CLI（`gh`）。
```

将上述内容放入 `.claude/commands/fix-github-issue.md` 使其在 Claude Code 中作为 `/project:fix-github-issue` 命令可用。然后您可以例如使用 `/project:fix-github-issue 1234` 让 Claude 修复问题 #1234。类似地，您可以将自己的个人命令添加到 `~/.claude/commands` 文件夹，用于您希望在所有会话中可用的命令。

3. 尝试常见工作流程
-----------------------

Claude Code 不强制特定的工作流程，给您使用它的灵活性。在这种灵活性提供的空间内，我们用户社区中出现了几种有效使用 Claude Code 的成功模式：

### a. 探索、计划、编码、提交

这种多功能工作流程适合许多问题：

1.   **要求 Claude 读取相关文件、图像或 URL**，提供一般指针（"读取处理日志记录的文件"）或特定文件名（"读取 logging.py"），但明确告诉它暂时不要编写任何代码。
    1.   这是工作流程中您应该考虑大量使用子代理的部分，特别是对于复杂问题。告诉 Claude 使用子代理来验证细节或调查它可能有的特定问题，特别是在对话或任务的早期，往往会保留上下文可用性，而在效率损失方面没有太多缺点。

2.   **要求 Claude 制定如何处理特定问题的计划**。我们建议使用"思考"这个词来触发扩展思考模式，这给 Claude 额外的计算时间来更彻底地评估替代方案。这些特定短语直接映射到系统中增加的思考预算级别："think" < "think hard" < "think harder" < "ultrathink"。每个级别为 Claude 分配逐渐增加的思考预算。
    1.   如果这一步的结果看起来合理，您可以让 Claude 创建一个文档或 GitHub 问题来记录其计划，这样如果实施（步骤 3）不是您想要的，您可以重置到这个点。

3.   **要求 Claude 在代码中实施其解决方案**。这也是要求它在实施解决方案的各个部分时明确验证其解决方案合理性的好地方。
4.   **要求 Claude 提交结果并创建拉取请求**。如果相关，这也是让 Claude 更新任何 README 或变更日志以解释它刚才所做的事情的好时机。

步骤 #1-#2 至关重要——没有它们，Claude 倾向于直接跳到编码解决方案。虽然有时这就是您想要的，但要求 Claude 首先研究和计划显著提高了需要前期深入思考的问题的性能。

### b. 编写测试，提交；编码，迭代，提交

这是 Anthropic 最喜欢的工作流程，用于可以通过单元、集成或端到端测试轻松验证的更改。测试驱动开发（TDD）在智能编程中变得更加强大：

1.   **要求 Claude 基于预期的输入/输出对编写测试**。明确说明您正在进行测试驱动开发，这样它就会避免创建模拟实现，即使对于代码库中尚不存在的功能也是如此。
2.   **告诉 Claude 运行测试并确认它们失败**。在这个阶段明确告诉它不要编写任何实现代码通常很有帮助。
3.   **当您对测试满意时，要求 Claude 提交测试**。
4.   **要求 Claude 编写通过测试的代码**，指示它不要修改测试。告诉 Claude 继续直到所有测试通过。Claude 通常需要几次迭代来编写代码、运行测试、调整代码并再次运行测试。
    1.   在这个阶段，要求它用独立的子代理验证实现没有过度拟合测试可能会有帮助

5.   **一旦您对更改满意，要求 Claude 提交代码**。

Claude 在有明确目标进行迭代时表现最佳——视觉模拟、测试用例或其他类型的输出。通过提供像测试这样的预期输出，Claude 可以进行更改、评估结果并逐步改进直到成功。

### c. 编写代码，截图结果，迭代

类似于测试工作流程，您可以为 Claude 提供视觉目标：

1.   **为 Claude 提供截取浏览器截图的方法**（例如，使用 [Puppeteer MCP 服务器](https://github.com/modelcontextprotocol/servers/tree/c19925b8f0f2815ad72b08d2368f0007c86eb8e6/src/puppeteer)、[iOS 模拟器 MCP 服务器](https://github.com/joshuayoes/ios-simulator-mcp)，或手动复制/粘贴截图到 Claude）。
2.   **通过复制/粘贴或拖放图像，或给 Claude 图像文件路径来为 Claude 提供视觉模拟**。
3.   **要求 Claude 在代码中实现设计**，截取结果的截图，并迭代直到其结果与模拟匹配。
4.   **当您满意时要求 Claude 提交**。

像人类一样，Claude 的输出往往通过迭代显著改善。虽然第一个版本可能不错，但经过 2-3 次迭代后通常会看起来好得多。为 Claude 提供查看其输出的工具以获得最佳结果。

![图片 2：安全 YOLO 模式](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F6ea59a36fe82c2b300bceaf3b880a4b4852c552d-1600x1143.png&w=3840&q=75)

### d. 安全 YOLO 模式

您可以使用 `claude --dangerously-skip-permissions` 绕过所有权限检查，让 Claude 不间断地工作直到完成，而不是监督 Claude。这适用于修复 lint 错误或生成样板代码等工作流程。

让 Claude 运行任意命令是有风险的，可能导致数据丢失、系统损坏，甚至数据泄露（例如，通过提示注入攻击）。为了最小化这些风险，在没有互联网访问的容器中使用 `--dangerously-skip-permissions`。您可以按照这个[参考实现](https://github.com/anthropics/claude-code/tree/main/.devcontainer)使用 Docker Dev Containers。

### e. 代码库问答

在接触新代码库时，使用 Claude Code 进行学习和探索。您可以向 Claude 询问在结对编程时会向项目中其他工程师询问的相同类型问题。Claude 可以智能地搜索代码库来回答一般问题，如：

*   日志记录是如何工作的？
*   如何创建新的 API 端点？
*   `foo.rs` 第 134 行的 `async move { ... }` 是做什么的？
*   `CustomerOnboardingFlowImpl` 处理哪些边缘情况？
*   为什么我们在第 333 行调用 `foo()` 而不是 `bar()`？
*   `baz.py` 第 334 行在 Java 中的等价物是什么？

在 Anthropic，以这种方式使用 Claude Code 已成为我们的核心入职工作流程，显著改善了上手时间并减少了其他工程师的负担。不需要特殊的提示！只需提问，Claude 就会探索代码来找到答案。

![图片 3：使用 Claude 与 git 交互](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2Fa08ea13c2359aac0eceacebf2e15f81e8e8ec8d2-1600x1278.png&w=3840&q=75)

### f. 使用 Claude 与 git 交互

Claude 可以有效处理许多 git 操作。许多 Anthropic 工程师使用 Claude 进行 90%+ 的 _git_ 交互：

*   **搜索 _git_ 历史**来回答诸如"v1.2.3 中包含了哪些更改？"、"谁拥有这个特定功能？"或"为什么这个 API 是这样设计的？"等问题。明确提示 Claude 查看 git 历史来回答这类查询会有帮助。
*   **编写提交消息**。Claude 会自动查看您的更改和最近的历史来编写考虑所有相关上下文的消息
*   **处理复杂的 git 操作**，如还原文件、解决变基冲突以及比较和移植补丁

### g. 使用 Claude 与 GitHub 交互

Claude Code 可以管理许多 GitHub 交互：

*   **创建拉取请求**：Claude 理解简写"pr"，并会基于差异和周围上下文生成适当的提交消息。
*   **为简单的代码审查评论实施一次性解决方案**：只需告诉它修复您 PR 上的评论（可选地，给它更具体的指示），完成后推送回 PR 分支。
*   **修复失败的构建**或 linter 警告
*   **通过要求 Claude 循环遍历开放的 GitHub 问题来分类和分流开放问题**

这消除了记住 `gh` 命令行语法的需要，同时自动化例行任务。

### h. 使用 Claude 处理 Jupyter notebooks

Anthropic 的研究人员和数据科学家使用 Claude Code 来读写 Jupyter notebooks。Claude 可以解释输出，包括图像，提供了一种快速探索和与数据交互的方式。没有必需的提示或工作流程，但我们推荐的工作流程是在 VS Code 中并排打开 Claude Code 和 `.ipynb` 文件。

您也可以要求 Claude 在向同事展示之前清理或对您的 Jupyter notebook 进行美学改进。具体告诉它使 notebook 或其数据可视化"美观"往往有助于提醒它正在为人类观看体验进行优化。

4. 优化您的工作流程
-------------------------

以下建议适用于所有工作流程：

### a. 在指令中要具体

Claude Code 的成功率通过更具体的指令显著提高，特别是在首次尝试时。预先给出明确的方向减少了后续需要纠正路线的需要。

例如：

| 差 | 好 |
| --- | --- |
| 为 foo.py 添加测试 | 为 foo.py 编写新的测试用例，涵盖用户注销时的边缘情况。避免模拟 |
| 为什么 ExecutionFactory 有这么奇怪的 api？ | 查看 ExecutionFactory 的 git 历史并总结其 api 是如何形成的 |
| 添加日历小部件 | 查看主页上现有小部件的实现方式以了解模式，特别是代码和接口是如何分离的。HotDogWidget.php 是一个很好的开始示例。然后，按照模式实现一个新的日历小部件，让用户选择月份并向前/向后翻页选择年份。从头构建，不使用代码库中已使用的库之外的库。 |

Claude 可以推断意图，但它不能读心术。具体性导致与期望更好的对齐。

![图片 4：给 Claude 图像](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F75e1b57a0b696e7aafeca1ed5fa6ba7c601a5953-1360x1126.png&w=3840&q=75)

### b. 给 Claude 图像

Claude 通过几种方法在图像和图表方面表现出色：

*   **粘贴截图**（专业提示：在 macOS 中按 _cmd+ctrl+shift+4_ 截图到剪贴板，按 _ctrl+v_ 粘贴。注意这不是您通常在 mac 上用来粘贴的 cmd+v，并且不能远程工作。）
*   **直接拖放**图像到提示输入
*   **提供图像的文件路径**

这在使用设计模拟作为 UI 开发的参考点以及用于分析和调试的视觉图表时特别有用。如果您没有向上下文添加视觉效果，明确告诉 Claude 结果的视觉吸引力有多重要仍然会有帮助。

![图片 5：提及您希望 Claude 查看或处理的文件](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F7372868757dd17b6f2d3fef98d499d7991d89800-1450x1164.png&w=3840&q=75)

### c. 提及您希望 Claude 查看或处理的文件

使用 tab 补全快速引用仓库中任何地方的文件或文件夹，帮助 Claude 找到或更新正确的资源。

![图片 6：给 Claude URL](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2Fe071de707f209bbaa7f16b593cc7ed0739875dae-1306x1088.png&w=3840&q=75)

### d. 给 Claude URL

在提示中粘贴特定 URL 供 Claude 获取和阅读。为了避免对相同域（例如，docs.foo.com）的权限提示，使用 `/permissions` 将域添加到您的允许列表。

### e. 及早且经常地纠正路线

虽然自动接受模式（shift+tab 切换）让 Claude 自主工作，但通过成为积极的协作者并指导 Claude 的方法，您通常会获得更好的结果。您可以通过在开始时向 Claude 彻底解释任务来获得最佳结果，但您也可以随时纠正 Claude 的路线。

这四个工具有助于路线纠正：

*   **要求 Claude 在编码前制定计划**。明确告诉它在您确认其计划看起来不错之前不要编码。
*   **在任何阶段按 Escape 中断** Claude（思考、工具调用、文件编辑），保留上下文，这样您可以重定向或扩展指令。
*   **双击 Escape 跳回历史**，编辑之前的提示，并探索不同的方向。您可以编辑提示并重复，直到获得您要寻找的结果。
*   **要求 Claude 撤销更改**，通常与选项 #2 结合使用以采取不同的方法。

虽然 Claude Code 偶尔在第一次尝试时完美解决问题，但使用这些纠正工具通常能更快地产生更好的解决方案。

### f. 使用 `/clear` 保持上下文聚焦

在长时间会话期间，Claude 的上下文窗口可能会填满无关的对话、文件内容和命令。这可能会降低性能，有时会分散 Claude 的注意力。在任务之间频繁使用 `/clear` 命令来重置上下文窗口。

### g. 对复杂工作流程使用检查清单和草稿本

对于具有多个步骤或需要详尽解决方案的大型任务——如代码迁移、修复大量 lint 错误或运行复杂构建脚本——通过让 Claude 使用 Markdown 文件（甚至 GitHub 问题！）作为检查清单和工作草稿本来提高性能：

例如，要修复大量 lint 问题，您可以执行以下操作：

1.   **告诉 Claude 运行 lint 命令**并将所有结果错误（带有文件名和行号）写入 Markdown 检查清单
2.   **指示 Claude 逐一解决每个问题**，修复和验证后再勾选并移至下一个

### h. 将数据传递给 Claude

有几种方法为 Claude 提供数据：

*   **直接复制粘贴**到您的提示中（最常见的方法）
*   **管道传输到 Claude Code**（例如，`cat foo.txt | claude`），特别适用于日志、CSV 和大数据
*   **告诉 Claude 通过 bash 命令、MCP 工具或自定义斜杠命令拉取数据**
*   **要求 Claude 读取文件**或获取 URL（也适用于图像）

大多数会话涉及这些方法的组合。例如，您可以管道传入日志文件，然后告诉 Claude 使用工具拉入额外的上下文来调试日志。

5. 使用无头模式自动化您的基础设施
-------------------------------------------

Claude Code 包含用于非交互式上下文（如 CI、预提交钩子、构建脚本和自动化）的[无头模式](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview#automate-ci-and-infra-workflows)。使用 `-p` 标志和提示来启用无头模式，使用 `--output-format stream-json` 进行流式 JSON 输出。

注意无头模式不会在会话之间持续。您必须在每个会话中触发它。

### a. 使用 Claude 进行问题分流

无头模式可以支持由 GitHub 事件触发的自动化，例如在您的仓库中创建新问题时。例如，公共 [Claude Code 仓库](https://github.com/anthropics/claude-code/blob/main/.github/actions/claude-issue-triage-action/action.yml)使用 Claude 检查新问题并分配适当的标签。

### b. 使用 Claude 作为 linter

Claude Code 可以提供传统 linting 工具检测不到的[主观代码审查](https://github.com/anthropics/claude-code/blob/main/.github/actions/claude-code-action/action.yml)，识别诸如拼写错误、过时评论、误导性函数或变量名等问题。

6. 使用多 Claude 工作流程提升水平
--------------------------------------

除了独立使用，一些最强大的应用涉及并行运行多个 Claude 实例：

### a. 让一个 Claude 编写代码；使用另一个 Claude 验证

一个简单但有效的方法是让一个 Claude 编写代码，而另一个审查或测试它。类似于与多个工程师合作，有时拥有单独的上下文是有益的：

1.   使用 Claude 编写代码
2.   运行 `/clear` 或在另一个终端中启动第二个 Claude
3.   让第二个 Claude 审查第一个 Claude 的工作
4.   启动另一个 Claude（或再次 `/clear`）来阅读代码和审查反馈
5.   让这个 Claude 基于反馈编辑代码

您可以对测试做类似的事情：让一个 Claude 编写测试，然后让另一个 Claude 编写代码使测试通过。您甚至可以通过给它们单独的工作草稿本并告诉它们写入哪一个和读取哪一个来让您的 Claude 实例相互通信。

这种分离通常比让单个 Claude 处理所有事情产生更好的结果。

### b. 拥有仓库的多个检出

许多 Anthropic 工程师做的事情是，而不是等待 Claude 完成每个步骤：

1.   **在单独的文件夹中创建 3-4 个 git 检出**
2.   **在单独的终端标签中打开每个文件夹**
3.   **在每个文件夹中启动 Claude**，执行不同的任务
4.   **循环检查**进度并批准/拒绝权限请求

### c. 使用 git worktrees

这种方法对于多个独立任务表现出色，为多个检出提供了更轻量级的替代方案。Git worktrees 允许您将同一仓库的多个分支检出到单独的目录中。每个 worktree 都有自己的工作目录和隔离的文件，同时共享相同的 Git 历史和 reflog。

使用 git worktrees 使您能够在项目的不同部分同时运行多个 Claude 会话，每个都专注于自己的独立任务。例如，您可能有一个 Claude 重构您的身份验证系统，而另一个构建完全不相关的数据可视化组件。由于任务不重叠，每个 Claude 都可以全速工作，而不必等待另一个的更改或处理合并冲突：

1.   **创建 worktrees**：`git worktree add ../project-feature-a feature-a`
2.   **在每个 worktree 中启动 Claude**：`cd ../project-feature-a && claude`
3.   **根据需要创建额外的 worktrees**（在新的终端标签中重复步骤 1-2）

一些提示：

*   使用一致的命名约定
*   为每个 worktree 维护一个终端标签
*   如果您在 Mac 上使用 iTerm2，[设置通知](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview#notification-setup)以便在 Claude 需要注意时收到通知
*   为不同的 worktrees 使用单独的 IDE 窗口
*   完成后清理：`git worktree remove ../project-feature-a`

### d. 使用带有自定义工具的无头模式

`claude -p`（无头模式）将 Claude Code 程序化地集成到更大的工作流程中，同时利用其内置工具和系统提示。使用无头模式有两种主要模式：

1. **扇出**处理大型迁移或分析（例如，分析数百个日志中的情感或分析数千个 CSV）：

1.   让 Claude 编写脚本生成任务列表。例如，生成需要从框架 A 迁移到框架 B 的 2k 文件列表。
2.   循环遍历任务，为每个任务程序化地调用 Claude，给它一个任务和一组可以使用的工具。例如：`claude -p "将 foo.py 从 React 迁移到 Vue。完成后，如果成功您必须返回字符串 OK，如果任务失败则返回 FAIL。" --allowedTools Edit Bash(git commit:*)`
3.   多次运行脚本并优化您的提示以获得所需的结果。

2. **管道化**将 Claude 集成到现有的数据/处理管道中：

1.   调用 `claude -p "<您的提示>" --json | your_command`，其中 `your_command` 是处理管道的下一步
2.   就是这样！JSON 输出（可选）可以帮助为更容易的自动化处理提供结构。

对于这两种用例，使用 `--verbose` 标志调试 Claude 调用可能会有帮助。我们通常建议在生产中关闭详细模式以获得更清洁的输出。

您使用 Claude Code 的技巧和最佳实践是什么？标记 @AnthropicAI，这样我们就能看到您正在构建什么！

致谢
----------------

作者：Boris Cherny。这项工作借鉴了更广泛的 Claude Code 用户社区的最佳实践，他们的创造性方法和工作流程继续激励着我们。特别感谢 Daisy Hollman、Ashwin Bhat、Cat Wu、Sid Bidasaria、Cal Rueb、Nodir Turakulov、Barry Zhang、Drew Hodun 和许多其他 Anthropic 工程师，他们宝贵的见解和使用 Claude Code 的实践经验帮助塑造了这些建议。