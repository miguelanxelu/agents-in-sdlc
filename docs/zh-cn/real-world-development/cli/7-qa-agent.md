---
title: "第 7 课 - 创建并使用质量保证（QA）智能体"
description: "创建质量保证（QA）自定义智能体，结合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到具有明确定义质量保证（QA）流程的自定义智能体中。

在本课中，你将：

- 了解自定义智能体如何与指令、技能和模型上下文协议（MCP）工具协作。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调此 QA 流程，并生成可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的自定义项各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 使用这些能力评估需求并报告发现。它们各司其职，也无需开启另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可重复使用的 QA 配置文件。该配置文件将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据议题和已商定的需求检查功能，遵循存储库指令，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它用通过、失败或受阻状态报告每项需求，并提供支持证据。修改实现代码前必须征求我的同意，不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 捕获了预期的 QA 工作流和边界。这样可避免智能体在你只想验证功能时进行不完整检查或过度修改。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选的，但添加后可以为智能体指定清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 会先检查需求、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了任何职责或边界，请先让默认智能体修改，再继续操作。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选对话并选择 QA，以便它使用已有的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中，但仍可使用上述命令启用。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   根据议题和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. QA 智能体会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加可重复使用的专用角色，并审查了它的工作。在本课中，你：

- 了解了自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查了可重复使用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查了它的发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
