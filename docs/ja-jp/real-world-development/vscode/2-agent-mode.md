---
title: "レッスン 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションを少し変更するだけでも、通常は複数のファイルを更新し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを探し、変更してテストを実行し、問題をデバッグし、変更漏れのファイルを見つけて、さらに修正するという流れを繰り返します。

そこで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に動作します。開発者と同じように既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode でサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で作業し、すばやくフィードバックを得ながら対話できます。cloud agent はチームのメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

このレッスンでは、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数のファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前のレッスンでは publisher ヘルパーを追加しました。ここでは Copilot Agent Mode を使って、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを開き、現在の機能を確認します。

この Web サイトは、ボードゲームを対象とするクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリケーションはビルド時にページを静的 HTML としてレンダリングします。ページのフロントマターから Drizzle ORM を使ってローカル SQLite データベースに直接クエリを実行します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、操作を加える場合は標準の DOM API を使う、小さくスコープされた Astro `<script>` を追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前のレッスンの後、`main` に戻っていることを確認して Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くダイアログが表示されたら、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択してサイトを開きます。

> [!NOTE]
> Codespace ターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Codespace で Web サーバーを利用するための非公開トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を含む詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要な箇所で説明します。

最初の Web サイトは動作しますが、新しい機能を追加して改善しましょう。プロジェクトをテンプレートから作成した際、Issue のバックログも自動的に作成されています。GitHub Copilot にバックログを表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は自律的に変更を計画、実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップを作成する際に Claude Sonnet 4.5 を使用したため、手順に記載しています。提案コードの一貫性を保ち、よい体験を得ることが目的ですが、LLM は確率的に動作するため、記載例と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使うことがありますが、同じタスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択してコマンドを実行し、すべての Issue を一覧表示します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーにとって役立つ順に、項目の優先順位も決めています。

## 指示ファイルを確認する

Copilot にコードを生成してもらう前に、作業のコンテキストとして使う指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [user interface (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - ファイル形式別の Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルを基準とする相対パスです。指示を再利用しやすい小さな単位に分割したり、例やテンプレートを示したりできます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 種類の変更が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルターの動作に対するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

コードベースにマージする前にテストを実行し、成功する必要もあります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面の例。](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. 関連するバックログ Issue に基づいて実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探し、データ層のヘルパー、UI、テストを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット。](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては、数分かかる場合があります。作業中にコード エディターで **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はありません。完了後、すべての変更に対して **Keep** または **Undo** を選択できます。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット。](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中にテストが失敗することがありますが、問題ありません。Copilot はエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![完了した Copilot Agent Mode のチャット セッションのスクリーンショット。](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数のファイルにまたがる変更を行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の探索演習 - ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。このレッスンで学んだ内容を使い、ページネーションを実装するよう Copilot に依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定するかを決めます。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。これは Copilot と作業する際の通常の流れです。

## 参考資料

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
