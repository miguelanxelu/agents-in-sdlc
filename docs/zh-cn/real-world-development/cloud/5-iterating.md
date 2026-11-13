---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本研讨会中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍访问功能。现在检查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前所述，加入生成式人工智能（AI）并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队启用代码审查前，先检查 GitHub Copilot 为文档、相关游戏功能和无障碍访问功能生成的建议。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 只有存储库读取权限，并且**只能**写入用于编写代码的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对模型上下文协议（MCP）服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含代码文档。你将使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看拉取请求（PR）时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部模型上下文协议（MCP）服务器。如有需要，可以[自定义或禁用云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等 Copilot 完成工作。这可能需要几分钟，可以稍作休息或回顾之前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释及其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时审查的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。正常情况下，单体 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，无法预先规定具体提示内容。可以要求 Copilot：

- 为每个代码文件添加注释头，简要说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 如何开始新会话并执行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为可审查状态，再合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队运行。正常情况下，单体 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，可以继续迭代云智能体的工作，例如要求调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，创建此任务时，你还没有更新自定义指令）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出要求，Copilot 会读取评论并启动新会话。

## 审查无障碍访问功能

最后，审查通过无障碍访问自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现时，重点留意：
   - 用于切换模式的用户界面（UI）组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表（CSS）或其他样式更改。
   - 无障碍访问属性，例如无障碍富互联网应用程序（ARIA）标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。正常情况下，单体 Astro 应用的项目检查会全部通过。此过程可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到该分支，手动测试无障碍访问功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常，并且刷新页面后设置仍然保留。

无障碍访问自定义智能体帮助 Copilot 按照无障碍访问最佳实践实现这些功能。如果发现无障碍访问方面的问题或改进点，可以像前面一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委派任务

云智能体可以帮助你处理真实的待办事项，让你专注于其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，练习这一工作方式。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境的练习。在这些课程中，你：

- **检查了存储库附带的自定义指令文件**，以便了解它们如何影响云智能体的输出。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**完成无障碍访问工作，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍访问会话。
- **审查并迭代云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体环境的练习。还可以继续探索其他环境，巩固本次练习的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

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
