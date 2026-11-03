---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，将议题要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中查看筛选体验。现在，将这些功能整合到一个自定义智能体中，并为其定义清晰的 QA 流程。

在本课中，你将：

- 了解自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并查看可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力来评估要求并报告发现。QA 配置不会取代它们，也不需要启动另一个会话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建一个可复用的 QA 配置。该配置将说明 QA 要执行哪些检查，以及必须遵循哪些界限。

1. 返回 Codespace，并确认筛选会话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先检查其配置，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免你只想验证功能时，配置不完整或范围过大的智能体擅自修改功能。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需项；`name` 是可选的，但添加后可以为智能体提供清晰的显示名称。
3. 阅读配置中的指令，确认 QA 会从需求开始，遵循存储库指令，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏了任何职责或边界，请先要求默认智能体修改，再继续操作。

## 根据议题运行 QA

Copilot CLI 会在会话开始时加载项目智能体。创建配置后，恢复同一个筛选会话并选择 QA，这样它就可以使用已有上下文中的议题和规划决策。

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 你刚创建的智能体可能不会显示在可用智能体列表中，但仍然可以使用上述命令启用。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体开始工作！
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你在工作流中添加了可复用的专家角色，并审查了它的工作。在本课中，你：

- 了解了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其发现。

现在，你已准备好实现功能、更新技能、创建 QA 配置、编写测试并生成验证报告。接下来，[将这些内容整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
