---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解你要它做**什么**，也了解你希望它**如何**完成。

本练习将：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及按路径生效的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 路径不同，本路径不会使用更改前后的提示进行实时对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以即时并排展示影响。后续练习会在审查云智能体创建的 PR 时展示此说明文件的作用。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层需要单元测试。
- UI 应使用深色模式并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件顶部都应有注释块，描述文件用途。

使用说明文件可确保 Copilot 按照这些实践处理任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好设置，帮助它理解编码风格和要求。可以指定编码约定、库和注释偏好，也可以创建适用于整个存储库的说明，或针对特定文件类型和任务的说明。

说明文件有两种：

- `.github/copilot-instructions.md`：Copilot 在存储库的**每次**请求中都会收到的单个文件。应包含与大多数请求相关的项目级上下文，例如技术栈、项目概览、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的说明，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本练习范围。示例项目展示了有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言，清楚地说明要求，并提供正反示例。

没有唯一正确的说明文件写法，可以通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件。此项目包含多个任务说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适当位置介绍。
>
> 需要模板或起点？请浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

先阅读项目提供的说明文件：一个核心 `copilot-instructions.md` 和一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下查看嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，注意有 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置相对于存储库根目录的 glob；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会使用这些说明。
5. 阅读项目的单元测试说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。说明文件可链接到其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件。路径相对于说明文件的位置，可借此复用较小的说明集并引用示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件头注释规则。

## 添加新的存储库级标准

接下来在 `copilot-instructions.md` 中添加一项项目级规则，让 Copilot 云智能体在之后处理议题时遵循该标准。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`，找到 **Code formatting requirements** 部分，通常在第 27 行附近。
3. 在现有标准下方添加以下 Markdown 内容：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可放在 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可放在 `*.instructions.md` 中，为特定语言、文件类型或任务提供指导。项目级文件适合存放文档注释等全局标准。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明文件。下一练习将从 `main` 分支开始，因此必须先把说明更新合并到 `main`，云智能体才能读取它。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样云智能体在下一练习处理议题时就能读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成本研讨会，可以跳过创建分支，直接提交到 `main`。这里使用分支是为了模拟真实项目的工作方式。

## 总结与后续步骤

你探索了 Copilot 如何从项目说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，云智能体创建的 PR 会展示这些说明的作用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
