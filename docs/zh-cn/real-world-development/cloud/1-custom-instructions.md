---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 不仅理解要做什么，也理解应该如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传达给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习环境不同，本练习不会比较提示的前后效果。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

和任何优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库以及注释风格等偏好，从而引导 Copilot 提供更相关的建议和代码片段。你可以创建适用于整个存储库的指令，也可以创建适用于特定文件类型或任务的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。该文件应包含项目级信息，即适用于大多数 Copilot Chat 或 CLI 请求的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件。例如，可为 TypeScript 或 Astro 等语言，或创建 UI 组件或单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中使用时，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

如何创建指令文件可以深入讨论，本研讨会不再展开。不过，示例项目展示了一种有代表性的做法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，让指导清晰易懂，并提供代码示例说明推荐或不推荐的写法。

创建指令文件并没有唯一正确的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目中的文件时，你可能会注意到，其中包含适用于多种任务的文件，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关练习会在适用时介绍这些操作。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花点时间阅读此存储库随附的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob，用于确定适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 留意本项目针对单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供可参考的示例。这些路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求在代码中添加文档注释。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的唯一编辑，是添加一项项目级规则，要求在代码中使用 TSDoc 文档注释并添加文件级注释头。稍后在本练习环境中分配议题时，云智能体会遵循这项规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议将更改整洁地提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来，为存储库编码标准添加文档说明，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应靠近第 27 行）。留意其中介绍项目编码标准的内容；目前没有要求在代码中添加文档注释，因此生成的辅助函数也没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以通过 `*.instructions.md` 文件为特定语言、文件类型或任务创建指令。项目级标准（例如刚添加的文档注释规则）适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。这样，在下一练习中处理分配给它的议题时，云智能体就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了让研讨会流程更贴近真实项目。

## 总结和后续步骤

你了解了 Copilot 如何从本项目的指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。在本练习环境剩余部分审查云智能体生成的拉取请求时，你会看到这项标准的实际效果。

接下来，[将你的第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
