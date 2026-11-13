---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的智能体，例如确保符合《Web 内容无障碍指南》（WCAG）标准的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 目录中定义。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML frontmatter，后面是用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是工作者，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并通过协调技能（甚至其他智能体）来完成工作。在本研讨会中，无障碍访问自定义智能体会根据无障碍访问指南审查并更新网站；执行任务时，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。和其他 AI 用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将介绍如何使用自定义智能体：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，当前暗色主题的文本与背景颜色对比度不足，导致部分用户难以阅读。为解决这一无障碍访问问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此需要尽快实现此功能。你将使用自定义智能体生成相关功能。

## 审查无障碍访问自定义智能体

已为无障碍访问创建了一个自定义智能体。现在审查其内容，了解它将如何指导 Copilot。

返回 codespace，然后审查无障碍访问自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍访问最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 创建并分配议题

Mission control 是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot 云智能体、监控任务，甚至重定向任务并提供更多指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开自己的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    我们需要为网站添加高对比度模式。用户应能通过切换开关启用或关闭高对比度模式。设置应保存在浏览器本地存储中。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中高亮显示自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。使用自定义智能体可以将团队的专业知识和标准整理成可重复使用的智能体，更有效地指导 Copilot 完成特定类型的工作。

本课介绍了以下内容：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，从 mission control [监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
