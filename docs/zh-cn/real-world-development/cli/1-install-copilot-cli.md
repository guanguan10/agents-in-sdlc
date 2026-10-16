---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉使用方式，并找到预置的筛选议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是功能强大的代理式编码助手，可在终端中运行，让你通过命令行探索代码库、生成代码、运行命令并使用外部工具。它可以分担任务、处理更改请求，并帮助你保持专注。第一步自然是安装工具；好在可以使用熟悉的工具来完成。

本课将：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 账户完成身份验证。
- 信任研讨会存储库并尝试一次简单对话。
- 使用内置 GitHub MCP 服务器查找筛选议题。

## 场景

团队开始使用 AI 智能体处理不断增长的待办事项。Copilot CLI 将这种能力带入终端，而终端是许多开发人员日常工作的主要环境。本课将完成安装和身份验证，为研讨会后续课程做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-cli]、WinGet 和 Homebrew 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本课将使用 npm。

1. 返回 Codespace 并打开终端。
2. 确认已安装 Node.js，且版本符合要求：

   ```bash
   node --version
   ```

   应显示版本 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   应显示版本号。

## 使用 GitHub 完成身份验证

首次启动 Copilot CLI 时，系统会提示使用 GitHub 账户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果出现提示，请按设备代码说明完成身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认路径是 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**。

> [!NOTE]
> 在 Codespace 中，可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有要求登录，就表示已准备就绪。

## 熟悉使用方式

普通 shell 提示符下的命令会直接在 Codespace 中运行。Copilot CLI 启动后，自然语言会发送给智能体，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，再按一次 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看已安装版本提供的命令，然后按 <kbd>Esc</kbd> 关闭帮助页面。
3. 向 Copilot 提出一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读响应，注意 Copilot 会先探索存储库再回答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 请 Copilot 查找筛选议题：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 阅读议题。完成首次小更改后会用到它。

> [!TIP]
> 普通 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次更改前都会创建功能分支。

## 使用研讨会快捷方式

通常，Copilot CLI 使用超出已授予权限范围的工具前会请求批准。本研讨会将使用 `--yolo` 重新启动 Copilot CLI。此快捷方式会在 Codespace 中移除这些批准提示，让你专注于练习。

> [!CAUTION]
> `--yolo` 会启用全部自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或 VM 等隔离环境中使用，绝不要将其设为日常开发的默认别名。详细信息请参阅[允许和拒绝工具使用][allow-all-warning]。

本研讨会还会使用 `--enable-all-github-mcp-tools` 启用后续课程中用于处理议题和拉取请求的 GitHub MCP 读写工具。Codespace 限制 Copilot 访问本地计算机，但已通过身份验证的 GitHub 资源仍是真实资源。发布或合并前请审查更改。

1. 输入 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再问一个关于项目的简单问题，确认对话正常，然后输入 `/exit` 退出。

Copilot 会自动保存对话。稍后更改说明或添加智能体后，可以使用 `copilot --resume` 返回同一功能对话和分支。

## 总结与后续步骤

恭喜！本课中，你：

- 使用 npm 安装了 GitHub Copilot CLI。
- 使用 GitHub 账户完成身份验证。
- 信任研讨会存储库并尝试了简单对话。
- 使用内置 GitHub MCP 服务器找到了筛选议题。

接下来，[开始第一个聚焦的更改][next-lesson]，让 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
