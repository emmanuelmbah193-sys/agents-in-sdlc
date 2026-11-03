---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]是提供代码生成指导的主要方式。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传达给 Copilot。
- 使用当前指令生成第一个筛选数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何应用新标准。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一项练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合设置的标准。Copilot 的行为具有非确定性。本练习的重点是观察更新指令后行为的总体变化，而不是要求输出逐字一致。

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

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。该文件应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
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

本项目已附带一组指令文件，其中包含一个核心 `copilot-instructions.md` 和多个适用于不同任务的 `*.instructions.md` 文件。在编辑器或 GitHub Web 界面中打开并浏览这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob，用于确定适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 留意本项目针对单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供可参考的示例。这些路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求在代码中添加文档注释。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 从上一项练习返回 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux)，打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先在 Copilot Chat 中尝试

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的会话。每个练习对模式和模型的选择可能不同，相关步骤会在适用时说明。

为了观察自定义指令的影响，先在当前指令文件保持不变的情况下发送提示。之后再更新指令并重新发送相同提示，比较结果。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这遵循 `.github/instructions/drizzle.instructions.md`（适用于 `src/lib/*.ts`）中的数据层约定。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系。本练习的重点仍然是观察更新指令后的一致性改进。

## 添加新的存储库标准

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

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除缓冲区。
2. 再次单击 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议文件现在是否以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议函数现在是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一项练习会在此基础上继续构建。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，通过源代码管理。接下来提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux)，打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求并将其关联到待办事项中的文档标准议题。也可以让 Copilot 使用已连接的 GitHub 工具代为完成：要求它查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 第一次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) 打开 Copilot Chat 视图，并确保智能体选择器中已选中 **Agent**。
3. 要求 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和发行商辅助函数基础：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先审查命令，再根据需要选择 **Allow**。
6. 创建 PR 后，使用以下提示要求 Copilot 合并 PR 并让本地分支返回 main：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再根据需要选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从本项目的指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何根据**现有**指令生成代码。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 重新发送相同提示，观察建议的代码如何应用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一项练习继续使用。

接下来，让 Copilot 在[智能体模式][next-lesson]中遵循指令，在整个代码库中添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
