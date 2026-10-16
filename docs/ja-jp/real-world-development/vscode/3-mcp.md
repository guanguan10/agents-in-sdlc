---
title: "レッスン 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使わせ、ブラウザーでフィルター機能を手動テストします。
- フィルター機能の作業を含むブランチを公開し、プルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディター外のツールに接続できると、さらに強力になります。Model Context Protocol (MCP) は Copilot が外部のツールやサービスと通信するための標準です。

![エージェント モードとコンテキスト、LLM、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) の相互作用を示す図。](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を使うことで、エージェントはリソースから最新情報を取得したり、ツールを使ってユーザーに代わって操作したりできます。

これらのツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービス (既存の API や NPM パッケージなどのローカル ツール) の間の通信を管理します。サーバーごとに、エージェントがアクセスできるツールやリソースが異なります。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスします。新しいリポジトリの作成、既存のリポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースを提供する MCP サーバーはほかにもあります。GitHub は、エコシステムの検索性と貢献を高めるための [MCP レジストリ](https://github.com/mcp)を提供しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱います。使う前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前のレッスンで Copilot がバックログの Issue を読めました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルでのインストールは不要です。VS Code が HTTP で接続し、初回利用時に GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。これを使って機能をテストします。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は **ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、ブラウザーを画面に表示せずに Playwright を実行することを指定します。デスクトップのない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストにすでに Playwright を使っているため、ブラウザーも通常インストール済みです。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて使用するときにサーバーが起動し、信頼するか確認されます。

1. `.vscode/mcp.json` で、`playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定をレビューし、サーバーの起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動して信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]があると、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して作業を確認する代わりに、エージェントにページを開き、移動し、フィルターを適用して結果を読み上げてもらい、確認内容を要約できます。会話を離れずに機能を検証できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、ボタン、リンク、リスト項目など、ラベル付きの構造化された要素をエージェントが判断します。支援技術と同じような要素に基づいて動作するため、簡単な機能確認がアクセシビリティの簡易チェックにもなります。

サーバーを接続し、アプリケーションを実行した状態で、次のプロンプトを Copilot に送信し、フィルター機能をテストしてもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ基準と照らし合わせて結果を読み、問題があれば追加の質問をしたり、プルリクエストを作成する前にコードの修正を依頼したりします。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使う際に、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が正しく動作することを確認できたので、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![Source Control パネルで変更を表示するスクリーンショット。](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンド ライン インターフェイス (CLI) から作成できます。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot が関連する Issue を探して、Issue に関連付けられた PR を作成できます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報の収集や操作を行うために求めた場合は、**Continue** を選択して許可します。GitHub ツールを初めて使う際は GitHub での認証を求められることがあります。画面の指示に従って許可します。
5. Copilot が Issue を検索して該当項目を見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択し、プルリクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP を使い、公開前にブラウザーでフィルター機能をテストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタム エージェント][next-lesson]を作成し、アクセシビリティ レビューなどに特化したタスクを効率化します。

## 参考資料

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
