---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在连续的 Tailspin Toys 工作流中使用 GitHub Copilot CLI。你完成了以下工作：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求（PR）。
- 从筛选议题开始，在计划模式中确定方案，在自动驾驶模式中构建，并在交互模式中审查。
- 使用自定义说明引导智能体，然后自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol（MCP）服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选功能 PR。
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，每个里程碑都基于更新后的 `main` 分支创建自己的分支：

1. **星级评分：**在游戏卡片上显示已有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为简化研讨会流程，这些工作都合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前综合检查这些结果、需求和测试覆盖范围。

添加的测试应弥补实际缺口；如果 QA 运行发现无需添加测试，也可能是合理结果。授权合并前，请审查代码和证据；代码更改后，应重新检查受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复任务，自定义智能体用于定义专业角色，MCP 服务器用于连接外部工具。应检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用**计划**模式先思考方案，使用**交互**模式参与聚焦的更改，使用**自动驾驶**模式处理范围明确的任务。日常编辑可选择速度较快的模型，复杂工作可选能力更强的模型。

基础设施固然重要，上下文同样关键。清楚说明要构建**什么**、**为什么**构建以及**如何**构建，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以了解其他 CLI 功能：

- 使用 `/review` 请代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取其他视角。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的最佳方式。可以将它用于生产代码、业余项目，或一直想构建却尚未动手的小应用。与团队分享经验并向他人学习，也别忘了探索文档。

如果想从终端扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，请浏览 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
