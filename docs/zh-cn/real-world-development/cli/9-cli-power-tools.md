---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供许多用于交互的斜杠命令。这些命令可访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来探索其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派到云智能体的可选命令。

## 场景

核心 CLI 工作流已经完成。现在可以了解其他功能，例如管理上下文、切换模型、共享会话，以及选择性地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；也可以通过斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及系统说明、工具、消息和剩余空间各自占用的上下文。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就较短，差异可能不明显。

> [!NOTE]
> 上下文窗口快满时，Copilot CLI 会自动压缩上下文。如果想自行选择时机，可使用 `/compact`。切换到无关任务并需要全新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也各有偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和使用信息。
3. 保持当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于所有人更好地使用 AI 工具。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中 `/share` 的选项。
2. 如果要共享会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 将导出内容发送给他人或发布前，请先审查内容。会话导出可能包含提示、响应和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还提供适用于大型任务的命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行运行。
- `/worktree` 可创建隔离的 Git 工作树来处理单独任务。
- `/delegate` 可将任务发送给 Copilot 云智能体，异步处理后可能会创建拉取请求。

这些命令是可选功能，因为它们可能会创建额外工作树或触发远程工作。试用前，请先开始一个范围明确的新任务，并通过常规工作流审查结果。如果想进一步了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

Copilot CLI 斜杠命令可用于配置工具、共享会话以及查看幕后工作。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派到云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。接下来，[回顾已学内容][next-lesson]并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
