---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，你让 Copilot 云智能体处理了三项任务，以改善用户体验并添加功能。虽然云智能体能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有拥有未关闭拉取请求（PR）的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

在本课中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。网站样式和切换功能已经在更新，因此一并加入这项功能很合理。你将引导智能体，在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表应包含分配给 Copilot 的任务，例如：
   - 更新代码库文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和处理方式而有所不同。

4. 留意右侧显示的拉取请求面板。在这里可以查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在你已了解正在进行的任务，可以请求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission control 中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。用户应能通过相应的切换开关选择所需的显示模式。
    ```

    ![智能体页面中的云智能体任务截图，其中高亮显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，然后将新请求加入工作流程。

这同样需要几分钟，可以借此暂停并回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过此任务控制中心，你可以跟踪所有进行中和已完成的任务、审查正在执行的工作，还可以重定向进行中的任务以调整范围或提供额外指导。

本课介绍了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加功能。

Copilot 正在完成无障碍访问功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
