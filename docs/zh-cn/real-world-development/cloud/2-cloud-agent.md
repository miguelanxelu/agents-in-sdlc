---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿拉取请求，供开发者审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发者专注于更重要的工作。

本课将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇创建网站的承包商没有留下完善的文档，事实上，网站完全缺少文档。团队希望先为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示同类别的其他游戏，让用户能够继续浏览。他们暂时不需要完善的实现，只需要可用于验收测试用户体验（UX）的版本。目前这项工作受阻，但其他议题优先级更高。

这些任务很容易被搁置，因此很适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者先完成其他工作，之后再回来审查 Copilot 的成果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法，使用它不代表可以跳过思考。看似简单的操作也可能包含复杂细节，因此要[认真界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为人工智能（AI）结对编程助手：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已经看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将任务分配给 Copilot 云智能体之前，先以只读方式浏览存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub Web 界面中打开以下文件；如果已有运行中的 codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述指令文件，后续课程审查拉取请求（PR）时还会再次提及。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给以类似软件工程师（SWE）方式工作的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，以便在智能体开始工作前运行，安装所需的开发工具和依赖项。本研讨会已预先配置此工作流，以便顺利完成练习并展示相关功能。它确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并植入数据的单体 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移本地 SQLite 数据库并植入数据，供构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个要点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 运行它。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务经常无人处理，会拖慢工作效率、增加代码库维护难度，也让新开发者更难加入团队。Copilot 擅长生成文档，因此很适合将这项工作交给云智能体。它会在后台生成所需文档，你可以在后续练习中审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    组织要求在适当情况下为函数和方法添加 TSDoc 文档注释。但近期更改没有遵循此标准。我们需要更新现有代码，确保在有助于说明行为时添加文档注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 创建的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建工作环境（运行前面介绍的工作流）、分析项目并确定合适的处理方法。

13. 审查列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码和理解状态、暂停思考并确定合适计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让开发者可以处理其他工作。接下来利用这一特性，再分配一项任务给云智能体，同时开始编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组任务。即使为设计团队添加相关游戏区域不需要很长时间，这段时间仍可用于处理其他工作。现在将这项任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。具体要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，但不包括该游戏本身。
   - 在游戏详情页显示一个使用该辅助函数的 "Related games" 区域。
   - 处理游戏没有相关游戏的情况。
   - 为新辅助函数创建单元测试。
   - 创建 PR 前，确保所有测试都通过。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需持续监控；等它完成后再审查即可。现在开始创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

本课介绍了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并能读取前几课中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
