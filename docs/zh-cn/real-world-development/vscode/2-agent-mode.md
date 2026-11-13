---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件，再次更新……后续还有许多工作。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式可以在集成开发环境（IDE）中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在体验如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，可快速反馈并与开发者交互。云智能体则像团队中的同事一样异步工作，通过议题和拉取请求与你交流。

在本练习中，你将了解如何：

- 让 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨用户界面、数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一课中已经添加了发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先了解它当前的功能。

Tailspin Toys 是一家带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看详细信息。该网站是一个单体 Astro 应用，在构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一课结束后，当前应位于 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的横幅，类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，在 Codespace 终端中选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前运行 Web 服务器的 codespace 的私有通道。

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **Home Page**：显示棋盘游戏列表，包括标题、图像和说明。
- **Game Details Page**：选择游戏后会打开详情页，其中包含标题、说明、发行商和类别等详细信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已可以正常运行，但我们希望添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题；现在让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此将其列入说明，以便尽量保持代码建议一致并提供良好体验。不过，大型语言模型（LLM）的输出具有概率性，你可能会得到与研讨会示例不同的建议。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项中的议题：

   ```plaintext
   请列出我的 GitHub 存储库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

留意 Copilot 还根据它认为对用户最有用的程度对议题进行了排序。

## 审查指令文件

让智能体生成代码前，先查看将用于为 Copilot 提供上下文的指令文件。本练习将使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。这些路径相对于指令文件所在位置。这样可以复用内容、将复杂指令拆分成更小且更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用进行三项不同的更新：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以执行这些任务，现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择了 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub 议题的要求，更新网站以支持按发行商和类别筛选。上一课中已经有一个发行商辅助函数，请保留并视需要完善它，同时完成剩余的数据层、UI 和测试工作。完成前确保所有测试都通过。服务器已经在运行，无需重新启动。
   ```

6. 观察 Copilot 探索项目并查找相关文件。它应会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。所需时间取决于任务和代码库的复杂程度，可能需要几分钟。过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 选项，因此无需在它工作时提前选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中要求用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现停顿，甚至有测试暂时失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表不断增长，网站需要支持分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示时提供 Copilot 所需指导，以便按预期实现功能。
- 可能需要通过提供更改请求和上下文与 GitHub Copilot 多次迭代。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
