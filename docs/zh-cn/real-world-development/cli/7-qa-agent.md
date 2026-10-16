---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 整合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，将这些能力整合到定义清晰的 QA 流程中，创建一个自定义智能体。

本课将：

- 探索自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义智能体可以协调此 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。该文件描述智能体的用途、说明和可用工具。本研讨会将创建 `.github/agents/qa.agent.md` 文件定义 QA 角色，并在 Copilot CLI 中选择该角色。

你已使用的自定义项各有不同用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何运用这些能力检查需求并报告发现。QA 配置不会取代这些能力，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建一个可复用的 QA 配置。该配置将定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选功能对话处于交互模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体之前，应先检查其配置，确认 Copilot 记录了预期的 QA 工作流和边界。这样可以避免智能体配置不完整或范围过宽，在只要求验证时擅自更改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需项；`name` 可选，但添加后会显示清晰的名称。
3. 阅读配置说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，在更改实现代码前征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏了任何职责或边界，请先让默认智能体修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置后恢复同一个筛选功能对话，再选择 QA，这样它就能使用已加载的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能尚未显示在可用智能体列表中。它已经存在，可以使用上述命令启用。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体开始工作。
4. 完成后，阅读它提供的报告。

## 总结与后续步骤

你为工作流添加了可复用的专业角色，并审查了其工作。本课中，你：

- 探索自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好进行审查。接下来，[将这些内容放入功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
