---
title: "第 3 课 - 智能体模式：计划与自动驾驶"
description: "使用计划模式商定方案，使用自动驾驶模式根据议题构建筛选功能，并在交互模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一项小功能。但较大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，帮助我们以正确的方式构建正确的内容。本课将开始一系列课程，介绍典型的智能体驱动开发流程：从议题开始生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一个对话和分支。通常，不同文件类型的工作会放在不同分支或 PR 中；这里为了聚焦核心概念，会采用简化流程。

本课将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在计划模式中定义需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成该功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行方筛选游戏。待办事项中描述了该功能，但组合类别等细节需要在编码前达成一致。你将使用计划模式明确这些决策，然后授权智能体在限定范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则；这些原则反而更加重要。大多数开发人员遵循的流程大致如下：

1. 从描述待办事项的已创建议题开始。
2. 制定构建内容的计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体步骤会因团队和组织而异，但大多数流程都与上述内容类似。

遵循标准流程，可以确保 AI 生成的代码满足既定需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式之间切换：

- **交互模式**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **计划模式**：智能体先制定计划，但不能编辑项目文件。
- **自动驾驶模式**：智能体自主工作，包括编写代码、运行测试并反复迭代，直至认为任务已完成。

先使用计划模式并审查计划，再切换到自动驾驶模式实现计划。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示并返回 **Session** 选项卡。

现在可以看到提示以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方法，再交由 Copilot 处理。复杂任务尤其适合花时间规划。切换到计划模式，并请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. 在过程中，Copilot 可能会询问筛选功能应如何工作。请根据自己的偏好作答，这里没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览计划内容。
6. 如果计划的任何部分与已作出的决定不符，请让 Copilot 修改。

## 批准自动驾驶

计划完成并经过审查后，就可以开始实现。使用自动驾驶模式让 Copilot 开始工作。

自动驾驶模式会让 Copilot 不断迭代，直到它认为问题已经解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

生成的代码与其他代码一样，需要在合并前审查。现在一起检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入交互模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题以及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决失败项。

## 探索新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行方筛选，以及计划中商定的组合。
4. 确认重置和无结果状态符合议题要求及相关决定。
5. 返回 Codespace，并请 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在计划模式中定义需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义说明][next-lesson]深入了解如何让代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的自动驾驶模式][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
