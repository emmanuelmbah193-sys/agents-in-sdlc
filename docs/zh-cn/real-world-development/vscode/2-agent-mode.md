---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程通常包括：找到所有必要文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……如此反复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但智能体模式和云智能体适用于不同的工作体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何在 UI 和数据层中实现新功能。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，你希望让用户按类别和发行商筛选游戏。上一项练习中，你已添加发行商辅助函数。现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先探索 Tailspin Toys，了解现有功能。

该网站是一个带有开发者主题的桌游众筹平台。用户可以浏览游戏列表并查看详情。网站是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能使用小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一项练习结束后，你应该已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux)，在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd> (Mac) 或 <kbd>Ctrl</kbd> (Windows/Linux)，然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，Web 服务器会在其中运行！

### 探索网站

网站运行后，可以探索其功能。主要页面包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详细信息页**：选择游戏后会进入详细信息页，其中显示游戏标题、说明、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的会话。每个练习对模式和模型的选择可能不同，相关步骤会在适用时说明。

网站的初始实现已可正常运行，但我们还想添加新功能。先查看待办事项列表。通过模板设置项目时，系统已自动创建议题列表。请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并修改多个文件、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此说明中列出了该模型。我们的目的是让建议代码相对一致，从而提供良好体验。不过，由于 LLM 具有概率性，你收到的建议可能与研讨会中的描述不同。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问 Copilot 有哪些待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还根据对用户的价值为议题排定了优先级。

## 查看指令文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。我们将使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了为网站添加功能的方法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件对如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向不同文件类型具体指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这样可以复用内容、将复杂指令拆分为较小且易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页并添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以为你完成这些任务！现在来添加此功能。

1. 可以继续当前 Copilot 会话，也可以选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，要求 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。具体耗时取决于任务复杂度和代码库大小，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此不必在工作进行期间选择它们。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认是否运行测试的截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再发现错误。

   ![包含 Copilot 智能体模式完整会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你了解了：

- GitHub Copilot 智能体模式如何在 UI 和数据层中实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 额外探索练习 – 实现分页

游戏列表不断增长后，需要添加分页。运用本练习中学到的技能，提示 Copilot 为网站实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页条目数，还是使用固定数量。
- 编写提示时，提供足够指导，让 Copilot 按照你的设想实现功能。
- 你可能需要通过提供上下文并要求更改来迭代改进。这是与 Copilot 协作的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
