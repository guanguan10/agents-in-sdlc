---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可自主执行更新代码和添加功能等任务。工作完成后，它会创建草稿 PR，供开发人员审查。这有助于委派繁琐任务、加快开发过程，并让开发人员专注于更重要的工作。

本练习将了解如何：

- 自定义生成代码的环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望处理一些技术债务。最初承包网站开发的团队没有留下完善的文档；现在需要为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现功能。他们希望游戏详情页显示相关游戏，即同一类别中的其他作品。此时不需要完善的最终实现，只要能用于用户体验验收测试即可。虽然这项工作目前受阻，但还有优先级更高的其他议题。

这些任务很容易被降级为低优先级，非常适合交由 Copilot 云智能体异步处理。开发人员可以先处理其他任务，之后再回来审查 Copilot 的工作并确认结果符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。工作进行期间或完成后，都可以查看 Copilot 会话。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明

GitHub Copilot 没有魔法，不能替你省略思考过程。即使看似简单的操作，深入分析后也会发现其复杂性，因此应注意[如何界定云智能体任务范围][cloud-agent-best-practices]。请把它视为 AI 配对程序员：分阶段工作、学习、试验并逐步调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

之前的练习介绍了自定义说明及其用途。如果已完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，本练习正好可以快速浏览。将任务分配给云智能体前，请以只读方式查看存储库中的说明文件，以便稍后观察其影响。

在 GitHub Web UI 或运行中的 Codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards**，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明应用于 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意 PR 中的 TSDoc 注释、TypeScript 约定和文件头注释；这些要求来自上述说明文件。后续练习审查 PR 时还会再次指出。

## 为 Copilot 云智能体设置开发环境

编写代码通常需要特定环境和设置脚本，以确保依赖项和工具可用。将任务交给 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 环境。可通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]自定义环境，使其能够使用所需开发工具和依赖项。本实验已预先配置好工作流，以便顺利完成练习。它会为单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果不在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点值得注意：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会执行此作业。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流，这有助于测试，而无需等待 Copilot 触发它。

## 添加文档

大多数项目要么文档过时，要么完全缺少文档。这类技术债务经常无人处理，会降低效率、增加维护难度，也会让新开发人员更难加入团队。Copilot 擅长生成文档，因此很适合负责此议题。它会在后台生成所需文档，后续练习将回来审查成果。

1. 在新的浏览器标签页中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，其标题可能类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请等待片刻并刷新浏览器。
12. 几分钟后，Copilot 应创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面提到的工作流）、分析项目并确定处理问题的方案。

13. 审查列表和 Copilot 将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并决定方案，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一就是异步执行任务，让开发人员可以处理其他工作。接下来利用这一点：再分配一个任务给云智能体，同时着手为应用添加功能。

## 在游戏详情页添加相关游戏

GitHub Copilot 云智能体的一项优势是可以拆分工作：你处理一项任务，它处理另一项任务。为设计团队添加相关游戏区域可能不会耗费太多时间，但也可以将这段时间用于其他工作。现在将此任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择空白议题模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论会显示 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二项请求。云智能体的工作方式类似软件工程师（SWE），因此无需主动监控；完成后再审查即可。接下来开始创建和使用自定义智能体。

## 总结与后续步骤

本课探索了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给 Copilot，让它异步执行任务，以处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了：

- 自定义生成代码的环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体在后台工作时，可以转而创建和使用自定义智能体。[云智能体也能使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
