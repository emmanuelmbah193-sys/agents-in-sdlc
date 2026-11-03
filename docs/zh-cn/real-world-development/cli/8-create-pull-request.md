---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能、自定义指令、技能更新、质量保证 (QA) 配置和测试都保存在同一个分支中。现在该一起审查并创建拉取请求了。之前你自行合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常应将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，我们将完整的筛选功能和质量工作流保留在同一个会话和分支中，并通过此 PR 一并提交。

在本课中，你将：

- 了解 Agent Merge 以及它如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时由开发人员掌握合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会逐项解决阻碍 PR 合并的问题，例如修复失败的持续集成 (CI) 检查、响应审查评论，以及在必要时变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

之前都是由你选择 **Merge pull request**。现在可以让 Agent Merge 负责此事。启用 Agent Merge 前，请先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码编写完成后，一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限不足问题。如果合并受阻，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。在本课中，你：

- 了解了 Agent Merge 以及它如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多有用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
