---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可提供此类指导，帮助 Copilot 理解你希望完成什么，以及应该如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义指令和按路径应用的指令文件提供给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，你会看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用指令文件可确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求，从而生成更相关的建议和代码片段。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型的任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，存储库中的**每个**请求都会发送给 Copilot。此文件应包含项目级信息，也就是大多数发送给 Copilot 的聊天或命令行界面（CLI）请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
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
> 使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目的文件时，你可能会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
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
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 中，注意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来决定指令适用于哪些文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 留意其中针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要进行唯一一次编辑：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后本环境的云智能体处理议题时会采用这项规则。

编辑前，先创建一个分支（云智能体会从议题目标分支读取指令，但你仍应整洁地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。记录存储库编码标准，有助于改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。留意它记录了项目编码标准，但尚无代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。对于刚添加的文档注释规则等项目级标准，存储库级文件是合适的位置。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中，云智能体处理议题时便会读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接操作 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何获取本项目的指令文件上下文，并在 `.github/copilot-instructions.md` 中添加了一项新的存储库级标准。接下来几课中，你会看到云智能体创建的拉取请求如何应用此标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
