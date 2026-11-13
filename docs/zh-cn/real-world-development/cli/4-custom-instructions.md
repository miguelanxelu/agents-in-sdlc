---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "浏览存储库指令，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能获取相关指导。[指令文件][instruction-files]不仅说明希望生成什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将探索 Copilot 使用的指令、添加文档标准，并将标准应用到代码中。

在本课中，你将：

- 了解存储库指令和按路径应用的指令文件如何提供给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数包含可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释应保持最新。
- 保留现有格式和 lint 指南。

使用指令文件可确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求，从而生成更相关的建议和代码片段。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型的任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，存储库中的**每个**请求都会发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

起始项目已附带一组指令文件。先浏览现有内容，再进行更改，以便观察更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。此存储库中与 Copilot 的所有交互都会应用这些指令。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用 glob 来决定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供可参考的示例。

## 更新指令文件以符合团队指导

现有文件是不错的起点，但仍有缺失。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为该部分的最后一条项目：

   ```markdown
   - 所有新编写的 TypeScript 代码都应包含用于文档说明的 TSDoc 注释。
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭对话。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff`，审查已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新标准应用于功能代码。具体来说，你：

- 了解了存储库指令和按路径应用的指令文件如何提供给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
