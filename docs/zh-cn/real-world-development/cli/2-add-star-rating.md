---
title: "第 2 课 - 添加星级评分：快速完成的小改进"
description: "使用 Copilot CLI 小幅更新游戏卡片，在转发的浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过一次对话后，现在可以开始对项目进行首次更改。先从小处着手：游戏数据中已经有星级评分，但主页上的游戏卡片尚未显示评分。请智能体显示评分、审查更改，然后将其作为第一个拉取请求合并。

本课将：

- 在功能分支中启动一个聚焦的 Copilot 对话。
- 请智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示该评分。不过，主页上的游戏卡片只显示标题、类别、发行方和说明。作为热身，让智能体在每张卡片上显示已有评分；这项小而独立的更改非常适合首次会话。

## 对话的组成

**对话**是使用 Copilot CLI 完成任务的工作空间。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话可供稍后继续使用，而文件和分支仍是磁盘上的普通 Git 状态。

对话中可以看到三类内容：提示和智能体的响应；智能体探索和编辑文件时的工具活动；以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

新建对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。现在已使用 Copilot CLI 添加了一项新功能。

## 审查差异

所有 AI 生成的更改在合并前都应经过审查，即使是小改动也不例外。现在就在 Copilot CLI 中检查这些更改。

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片在有评分时显示数字评分，`starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 审查 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果具有概率性而非确定性，因此具体代码可能有所不同。请检查行为，不要期待某一种固定实现。

## 检查更改

不要只阅读代码就假设它能够正常工作。请 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五星制评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已经完成，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你已使用 GitHub Copilot CLI 交付首项更改，具体包括：

- 在功能分支上启动聚焦的 Copilot 对话。
- 指导智能体对游戏卡片进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认星级评分。
- 创建并合并第一个拉取请求。

接下来，你将[从筛选议题开始，使用计划和自动驾驶模式][next-lesson]构建更大型的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
