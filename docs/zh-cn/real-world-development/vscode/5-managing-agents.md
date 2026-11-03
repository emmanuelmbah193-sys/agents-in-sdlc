---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主工作：探索代码库、提出更改、编辑文件并运行命令。由于 Copilot Chat 在 VS Code 中实时执行这些操作，你可以查看每次工具调用和文件编辑。你可以直接在聊天中查看每个差异、接受或拒绝单项更改，也可以通过后续提示引导会话以改进或扩展工作，而无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍访问智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间指导智能体，在高对比度模式之外添加浅色模式开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍访问自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式开关。你无需从头开始新会话，而是指导现有会话扩展工作范围。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异的形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容并查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在此会话中修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式的实现。完成后应会显示更改摘要。

## 指导智能体添加浅色模式

高对比度模式已实现，现在可以在同一会话中添加浅色模式开关。由于继续使用同一聊天会话，智能体会保留之前工作的上下文，包括修改的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会从之前的工作继续，探索现有更改并提出浅色模式的新增内容。
3. 和高对比度模式一样，逐项审查新编辑。对每项差异使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期（例如尝试以你不喜欢的方式重构开关 UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 指导正在进行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS，改用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，请将浅色模式整合到现有设置面板中”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认实现符合自定义智能体中的无障碍访问模式，包括 ARIA 属性、键盘导航、语义 HTML，以及使用本地存储持久化设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并指导正在进行的会话：查看每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课介绍了以下概念：

- 实时监控智能体的工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示指导正在进行的会话。

在[下一课][next-lesson]中，你将把无障碍访问功能作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
