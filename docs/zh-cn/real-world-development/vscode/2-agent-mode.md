---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并运行测试。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次修改，工作往往会不断重复。

这时可以使用 Copilot 智能体模式。

Copilot 智能体模式可以在 IDE 中更自主地工作。它会像开发人员一样先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在就来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，智能体模式和云智能体面向两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、找到相关文件并协调更改。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增加，因此希望用户可以按类别和发行方筛选。上一练习已添加发行方辅助函数，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改前先探索 Tailspin Toys 网站，了解它当前提供的功能。

该网站是一个以开发者为主题的桌游众筹平台，可以列出游戏并显示详情。它是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。请在 GitHub Codespace 中按以下步骤运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 Codespace 的私有隧道，开发服务器现在运行在其中。

### 探索网站

网站运行后，可以探索其功能，主要包括：

- **主页**：显示桌游列表及其标题、图片和说明。
- **游戏详情页**：选择游戏后进入详情页，其中会显示游戏标题、说明、发行方和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异，相关步骤会在适用处说明。

网站的初始实现已可运行，但仍可添加新功能。先查看项目待办事项。通过模板创建项目时，系统已自动创建一组议题；请 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写课程时使用了 Claude Sonnet 4.5，因此将其列入说明，希望能让代码建议保持相对一致。不过，LLM 具有概率性，建议可能与课程所述不同，这是正常且预期的现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，询问 Copilot 有哪些待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据对用户的价值为待办事项排序。

## 查看说明文件

让智能体生成代码前，先查看将为 Copilot 提供工作上下文的说明文件。本练习将使用[用户界面（UI）说明][ui-instructions]，其中包含为网站添加功能的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其总体指导，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向特定文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件的位置，因此可以复用内容、拆分复杂说明，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要三项应用更新：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以帮助完成这些任务。

1. 可以在当前 Copilot 对话中继续，也可以选择 **New Chat** 开始新对话。
2. 依次选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择 **Agent**。
4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目、查找相关文件、定位数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项操作，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 按钮，无需在工作进行时提前选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能有测试暂时失败。这没有关系：Copilot 会反复生成代码并运行测试，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了：

- 如何使用智能体模式跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、找到相关文件并协调更改。
- 合并前如何审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习：实现分页

随着游戏列表增大，需要支持分页。使用本练习学到的技能，请 Copilot 更新网站以实现分页。需要考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页方式，例如让用户选择每页条目数，或使用固定值。
- 编写提示时提供必要指导，以实现预期行为。
- 可能需要与 GitHub Copilot 迭代，通过后续请求和上下文调整结果。这是正常工作方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：它们的作用及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
