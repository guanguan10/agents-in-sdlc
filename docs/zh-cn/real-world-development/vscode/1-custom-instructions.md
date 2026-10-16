---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

上下文对于生成式 AI 至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]是提供代码生成指导的重要方式。

本练习将：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及按路径生效的说明文件传递给 Copilot。
- 使用现有说明生成筛选功能的第一个数据片段（发行方辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选功能片段提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合设置的标准，因为 Copilot 的结果具有概率性。本练习的重点是观察更新说明后的行为趋势，而不是逐字匹配输出。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层必须始终有单元测试。
- UI 应使用深色模式并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件顶部都应有一段注释，描述文件用途。

使用说明文件可以向 Copilot 提供正确的信息，确保它按照这些实践完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好设置，帮助它理解编码风格和要求。可以指定编码约定、库以及注释偏好。说明可以适用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：Copilot 在该存储库的**每次**请求中都会收到的单个说明文件，应包含项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

有关创建说明文件的完整讨论超出本练习范围。示例项目展示了有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，并通过示例清晰说明应如何或不应如何编写代码。

说明文件没有唯一正确的写法。可以通过试验找出适合项目的方式。

> [!TIP]
> 使用 GitHub Copilot 的项目都应有完善的说明文件。此项目为多种任务提供说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面操作各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适当位置介绍。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

先阅读存储库提供的说明文件：一个核心 `copilot-instructions.md` 和一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还包含 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，注意 Astro、Drizzle 数据层、测试等相关说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置相对于存储库根目录的 glob，决定文件适用范围，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 阅读项目的单元测试说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。说明文件可以相互引用，也能链接到项目现有文件；这些路径相对于说明文件所在位置。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来会添加 TSDoc 注释和文件头注释要求。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异，相关步骤会在适用处说明。

先在当前说明下发送提示，稍后更新说明后再次发送相同提示，以观察差异。

1. 关闭之前练习留下的编辑器标签，只保留所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，允许 Copilot 应用文件更改。
4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意建议的辅助函数是带类型的函数：第一个参数是 `db` 客户端，并返回带类型的发行方数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件头注释。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；本练习的重点仍是观察更新说明后行为的一致性变化。

## 添加新的存储库级标准

`.github/copilot-instructions.md` 用于提供项目级信息。接下来添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录项目编码标准，但还没有代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可以放在存储库级 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可以放在 `*.instructions.md` 中，为特定语言、文件类型或任务提供指导。新添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在建议的文件顶部会有类似以下注释：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意函数现在会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于下一练习会用到的实际代码。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，需要通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并关联待办事项中的文档标准议题。Copilot 可以找到议题，创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具接入工作区，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按提示授权即可。后续课程将介绍此连接如何通过 Model Context Protocol 工作。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确认已选中 **Agent**。
3. 请 Copilot 找到更新说明文件相关的议题，并从当前分支创建 PR，描述说明更新和发行方辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 系统提示允许 Copilot 代表你执行 GitHub 操作时，请审查命令并按需选择 **Allow**。
6. PR 创建后，使用以下提示请 Copilot 将 PR 合并并将当前分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示允许 Copilot 执行 GitHub 操作和 shell 命令时，请审查命令并按需选择 **Allow**。

现在已经借助 GitHub Copilot 创建并合并了一个拉取请求。

## 总结与后续步骤

你探索了 Copilot 如何读取项目说明，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有说明下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送同一提示，观察生成的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，Copilot 将在[智能体模式][next-lesson]中使用这些说明，跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
