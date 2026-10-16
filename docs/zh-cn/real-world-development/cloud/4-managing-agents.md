---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你要求 Copilot 云智能体分别处理三项改进用户体验和添加功能的任务。云智能体虽然能够自主异步工作，但监控任务仍很重要。

你可以通过多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，可以查看所有有未关闭 PR 的智能体任务，探索已执行的操作，甚至在会话进行中引导智能体。

本课将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此增加该功能很合理。现在引导智能体，同时实现浅色模式和高对比度模式。

## 审查 Copilot 云智能体任务

查看分配给 Copilot 云智能体的任务当前状态：

1. 访问 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 为代码库更新文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，审查 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 工作状态和所采用的方法而有所不同。

4. 同时注意右侧的拉取请求（PR）面板，可在其中查看 PR 和已更改文件，以便进一步监控。

## 引导云智能体

现在已了解正在执行的任务，可以请 Copilot 在实现高对比度模式时同时添加浅色模式开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。出现类似下方的消息时，表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示“Steer active session while Copilot is working”对话框的任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会处理更新后的任务。收到消息时，如果它正执行某个步骤，会在完成该步骤后将新请求纳入工作流程。

这同样需要几分钟，可以借此回顾迄今为止学到的内容。

## 总结与后续步骤

本课探索了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过此 mission control，可以跟踪所有活动和已完成的任务、审查正在执行的工作，并重定向进行中的任务以调整范围或提供指导。

本课介绍了：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在进行的会话以请求添加功能。

Copilot 正在完成无障碍功能工作。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
