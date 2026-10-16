---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ課題、更新が必要なレガシ コード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業を終えると、担当者がレビューできるドラフトのプルリクエスト (PR) を作成します。手間のかかるタスクを委任して開発を加速し、開発者がより大きな課題に集中できるようになります。

このレッスンでは、Copilot cloud agent を使って次のことを確認します。

- コード生成のために環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。最初のサイトを作った開発者は、ドキュメントがまったくない状態で作業を終えました。まず、アプリケーションのすべての公開関数に TSDoc のドキュメント コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリのほかのゲームを表示し、ユーザーがさらに閲覧できるようにします。まだ完成度の高い実装は必要ありません。UX の受け入れテストに使えるものを求めています。これは現在のブロッカーですが、優先度の高い Issue がほかにもあります。

どちらも優先度が下がりやすいタスクであり、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業している間、開発者は別のタスクに集中し、後で Copilot の作業をレビューして、期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行します。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ることができます。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境をセットアップしてタスクに取り組みます。作業中や完了後にセッションを確認できます。提案された解決方法のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見簡単な操作でも、詳細を確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーのように段階的に作業し、学習と実験を通じて進め方を調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot をガイドする方法を確認しました。カスタム指示の演習を完了していれば `.github/copilot-instructions.md` の動作を見ています。まだの場合は、ここで簡単に確認します。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後で効果を調べます。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを読み、特に TypeScript の規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。`**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されるか確認します。これらの指示ファイルの効果は、後の演習で PR をレビューする際にも確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は特定の環境を用意してセットアップ スクリプトを実行し、環境が正常であることを確認します。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェア エンジニアと同じような方法で作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用セットアップ ワークフロー][setup-workflow]を作成すると、cloud agent が作業を始める前に必要な開発ツールや依存関係を利用できるよう環境をカスタマイズできます。このラボ用にあらかじめ設定されており、単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションと初期データ投入済みの SQLite データベースが用意されます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最低限のアクセス許可を設定します。Copilot には操作用の別のトークンが付与されます。
    permissions:
      # たとえば依存関係をインストールするために、セットアップ手順でリポジトリをクローンする場合は `contents: read` が必要です。セットアップ手順でクローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
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

      # ローカル SQLite データベースをマイグレーションして初期データを投入し、ビルドとテストでデータを使えるようにします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフロー ファイルと似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の開始を待たずにワークフローの成功を確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く知られていますが、多くのプロジェクトで情報が古かったり、まったくなかったりします。こうした技術的負債は後回しにされがちで、生産性を低下させ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成された `[WIP]: Code lacks documentation` のようなタイトルのプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、タスクの最適な進め方を検討しています。

13. リストと実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を把握し、考えて実装計画を決めて、コードを作成する様子が表示されます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期でタスクを実行し、ほかの作業に集中できるようにすることです。この機能を活用して、別のタスクも cloud agent に割り当てた後、アプリケーションの機能を追加するコードを見ていきます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは作業を分担できることです。デザイン チーム向けの関連ゲーム セクションを追加する作業はそれほど時間がかからないかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. 空白のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が目の絵文字で Issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目のリクエストに取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。作業が完了してからレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で作業させることができます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに Copilot を利用できます。

このレッスンでは、次のことを確認しました。

- コード生成のために環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## 参考資料

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
