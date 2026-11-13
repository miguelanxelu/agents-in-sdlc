---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认它在浏览器中正常运行。无需手动浏览应用，你将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

在本课中，你将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright 模型上下文协议（MCP）服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当智能体模式可以访问编辑器以外的资源时，它的能力会大幅增强。MCP 是 Copilot 与外部工具交互的标准方式。

![智能体模式的工作原理示意图，以及它如何与上下文、大型语言模型（LLM）和工具交互，其中包括模型上下文协议（MCP）服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为人工智能（AI）智能体提供了与外部工具和服务通信的方式。使用 MCP 后，AI 智能体可以实时与外部工具和服务通信，从而通过资源获取最新信息，并代表你使用工具执行操作。

通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有应用程序编程接口（API）或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的应用程序编程接口（API），让 AI 智能体能够创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体能够浏览网页、填写表单和选择按钮。

此外还有许多提供不同工具和资源的 MCP 服务器。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 安全方面应将 MCP 服务器视为项目的其他依赖项。使用前，仔细审查源代码、验证发布者并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目由项目模板提供，因此上一课中的 Copilot 能够读取待办事项中的议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过超文本传输协议（HTTP）连接到该服务器，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控浏览器，用于测试刚完成的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此 Playwright 所需的浏览器通常已安装。如果 Copilot 随后报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请审查配置并选择信任，使服务器可以启动。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可驱动的真实浏览器。智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察到的情况，无需你离开对话手动浏览应用。这是在不中断工作流的情况下确认功能行为是否符合预期的快捷方式。

Playwright MCP 服务器会根据页面的[无障碍访问树][playwright-mcp-server]运行，而不是使用屏幕截图。智能体会处理带标签的结构化元素（按钮、链接和列表项），与辅助技术处理内容的方式类似，因此快速功能检查也能兼顾基本的无障碍访问检查。

服务器已连接且应用正在运行时，向 Copilot 请求测试刚构建的筛选功能：

```text
使用 Playwright MCP 服务器打开浏览器，访问正在运行的应用 http://localhost:4321，并验证新的游戏筛选功能：

1. 打开游戏页面并记录列出了多少款游戏。
2. 应用类别筛选条件，确认列表只显示该类别的游戏。
3. 清除筛选条件，然后应用发行商筛选，确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选，确认结果同时满足两个条件。

报告每一步的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，执行每一步并报告结果。根据议题中的验收标准审查总结；如果发现异常，提出后续问题，或在创建拉取请求前让 Copilot 修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）等多种方式创建拉取请求。你已经在使用 GitHub Copilot，因此可以让它创建 PR，并关联到相关议题。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请求 Copilot 创建 PR：

   ```plaintext
   找到存储库中与按类别和发行商筛选相关的议题。为当前分支创建拉取请求，并关联正确的议题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证；按照提示完成操作。
5. 留意 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本课中你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建了拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍访问审查等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
