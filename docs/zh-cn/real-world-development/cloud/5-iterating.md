---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 处理了多个任务，以改善用户体验并添加功能。你要求 Copilot 为代码添加文档、构建供设计团队迭代改进的相关游戏功能，以及实现包括高对比度和浅色模式开关在内的无障碍访问功能。现在，我们来检查代码更改，并在需要时向 Copilot 提供反馈，改进其工作。

### 场景

正如前面多次提到的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队其他成员启用审查之前，先审查 GitHub Copilot 为添加文档、相关游戏功能和无障碍访问功能创建的建议。

## 安全性和 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立、临时的工作环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 创建的第一个拉取请求（PR），其中包含为代码添加的文档。你将使用 GitHub.com 标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问包括外部 MCP 服务器调用在内的外部资源。你也可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 在新的浏览器选项卡中返回存储库的 github.com 页面。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或者描述更完整的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加了 TSDoc 文档注释和其他文档的代码。具体更改会有所不同。

   浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向沟通。你可以像在拉取请求中联系团队其他成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新的会话来处理它们。由于结果具有非确定性，我们无法提供唯一的提示文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，像提及其他用户一样提及 **@copilot**，要求它更改生成的文档。可以使用上面的任一建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 执行更改。留意 Copilot 如何启动新会话来完成更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

你可以继续迭代，直到满意为止。之后，可以将 PR 从草稿转换为准备审查状态，并合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 为解决“在游戏详细信息页面显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或者描述更完整的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查会通过。这可能需要几分钟。
9. **可选：**你也可以在 codespace 中切换到此分支，手动测试相关游戏功能。前往 codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，你可以继续迭代并要求 Copilot 更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令！）。和之前一样，可以在 **Conversation** 选项卡中发表评论，Copilot 会看到并启动新的会话。

## 审查无障碍访问功能

最后，审查通过无障碍访问自定义智能体实现的功能。这个 PR 应包括练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或者描述更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍访问属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查会通过。这可能需要几分钟。
10. **可选：**你也可以在 codespace 中切换到此分支，手动测试无障碍访问功能。前往 codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，确认功能正常，并且页面重新加载后仍会保留设置。

请注意，无障碍访问自定义智能体如何引导 Copilot 遵循无障碍访问最佳实践来实现这些功能。如果发现无障碍访问问题或可改进之处，可以像在前几个 PR 中那样，在评论中提及 **@copilot** 并要求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理真实的待办事项，让你能同时专注于其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，培养这种工作习惯。例如：

- 在游戏详细信息页添加支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体练习环境。在这些课程中，你：

- **检查了此存储库随附的自定义指令文件**，以便之后了解它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**，实现无障碍访问功能，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并指导无障碍访问会话。
- **审查并迭代改进云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体练习环境。如需继续学习，其他练习环境可以补充本次实践的内容：

- 🖥️ **[VS Code 练习环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习环境](../../cli/)** — 在终端中交付两个经过审查的更改，学习 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接功能（连接到云智能体）。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
