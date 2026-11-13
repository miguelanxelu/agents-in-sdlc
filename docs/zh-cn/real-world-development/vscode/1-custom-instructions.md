---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可为代码生成提供此类指导。

在本练习中，你将：

- 了解存储库自定义指令和按路径应用的指令文件如何向 Copilot 提供项目专属上下文、编码准则和文档标准。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 使用相同提示重新生成代码，观察新标准如何影响生成结果。
- 将指令更新和筛选功能数据切片提交到 `main`，供下一课使用。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有概率性。本练习要观察更新指令后行为的整体变化，而不是要求输出与示例完全相同。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用指令文件可确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求，从而生成更相关的建议和代码片段。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型的任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，存储库中的**每个**请求都会发送给 Copilot。此文件应包含项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建。你可以为 TypeScript 或 Astro 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在集成开发环境（IDE）中使用 Copilot 时，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

本研讨会不详细介绍如何创建指令文件，但示例项目展示了一种有代表性的方法。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（例如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明正确和错误的写法。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目、环境和场景的做法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目时，你会看到针对多种任务的指令，例如[用户界面（UI）更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应环境的课程会在相关处说明。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件，其中包含一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 中，注意嵌套的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来决定指令适用于哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些指令。
5. 留意其中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供生成代码时可参考的示例。这些路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

先创建分支，再进行更改。

1. 返回上一课中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

先在当前指令文件下发送提示，再更新指令并重新发送相同提示，以观察自定义指令的影响。

1. 关闭之前练习打开的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改辅助函数文件及其测试。
6. 留意生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习重点仍是观察更新指令后行为的一致性变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项存储库级编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。它记录了项目编码标准，但尚无代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出函数都应包含 TSDoc 注释，说明用途、参数和返回值。
   - 在导入语句或任何代码之前，为文件添加注释块，说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。对于刚添加的文档注释规则等项目级标准，存储库级文件是合适的位置。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

4. 留意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商及其 ID 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中。下一课会在此基础上继续构建。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，使用相同的源代码管理流程进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也将这些文件加入暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求，并关联待办事项中的文档标准议题。可以手动操作，也可以让已连接项目 GitHub 工具的 Copilot 查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成操作。稍后会在另一课中了解此连接如何通过模型上下文协议（MCP）工作。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 要求 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和为后续筛选工作添加的发行商辅助函数基础：

   ```plaintext
   查找与更新指令文件相关的议题。从当前分支创建拉取请求，说明此 PR 会关闭该议题，并注明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 提示你允许它代表你执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将分支切回 main：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以保持同步。
   ```

7. 如果 Copilot 提示你允许它执行 GitHub 操作和 shell 命令，请审查操作并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何获取指令文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在现有指令下的输出。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一课使用。

接下来，使用[智能体模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
