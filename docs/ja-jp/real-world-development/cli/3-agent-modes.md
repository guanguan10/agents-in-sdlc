---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue に基づくフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて使えるため、適切な方法で必要な機能を構築できます。このレッスンから始まる一連のレッスンでは、Issue から新機能を作り、コードの妥当性と機能の動作を確認し、最終的にプロジェクトへマージする、一般的なエージェント主導の開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常は変更するファイルの種類ごとに別のブランチや PR を作りますが、ここでは主要な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot 会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能の説明がありますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan モードで判断事項を整理し、範囲を限定して Autopilot に実装を許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本原則は変わりません。むしろ、さらに重要になります。多くの開発者は、次のような手順で作業します。

1. 実装内容を記述した Issue から始めます。
2. 実装内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合は上記の手順を基にしています。

この標準的な方法に従えば、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**によって、エージェントの自律性の程度が変わります。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: タスクが完了するまで、エージェントがコードを作成し、テストを実行して反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止中の場合は再起動してから続行します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` などが表示されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、計画に少し時間をかけることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な実装方法を検討して計画を作成します。

3. フィルター機能の動作について Copilot から質問された場合は、希望に基づいて回答します。どの答えも間違いではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 決定内容と異なる箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたので、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、タスクが完了したと判断するまで Copilot が反復して作業します。

1. **Accept plan and build on autopilot (recommended)**、またはインストールされているバージョンで同様の名前の項目を選択します。
2. プロンプトの下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に従って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot から依存関係の不足やポートの競合が報告された場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードを生成した後、ほかのコードと同様に、マージ前にレビューする必要があります。コードを確認し、サイトを実行して動作を確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を実行し、フィルター機能の実装とテストを調べます。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードはよさそうですが、実際に動作するでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発行元、およびその組み合わせで絞り込みを試します。
4. リセット時と結果がない場合の動作が、Issue と決定内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

さまざまな会話モードを使って、機能を実装してレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot 会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが記載された手法に従うよう、[カスタム指示を使って][next-lesson]さらに詳しく確認します。

## 参考資料

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
