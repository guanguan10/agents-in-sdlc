---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム エージェント][previous-lesson] |
|:--|

前の 2 つの演習では、ユーザー体験の改善や機能追加を目的に、GitHub Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期に自律して動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。この Mission control では、開いているプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行した操作を確認し、進行中のセッションに指示を送ることもできます。

このレッスンでは、次のことを行います。

- エージェント ページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに指示を送って、機能の追加を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加するよい機会だと気付きました。サイトのスタイルを更新して切り替え機能を追加する作業がすでに進んでいるため、同時にライト モードも実装するのが合理的です。ハイ コントラストに加えてライト モードも追加するよう、エージェントの作業に指示を送ります。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方でタスクの一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードのドキュメントの更新。
   - ゲームを変更する API の生成。
   - Web サイトへのハイ コントラスト モードの追加。
3. 実行中のタスクを 1 つ選択し、Copilot が行った作業を確認します。次の操作が含まれる場合があります。
   - リポジトリからのコードのチェックアウト。
   - Copilot が作業する環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するための各種作業。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。追加の監視に使う PR と変更ファイルが表示されます。

## Cloud agent に指示を送る

進行中のタスクを確認できたので、エージェントがハイ コントラスト モードに取り組んでいる間に、ライト モードの切り替えも実装するよう依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。正確なタイトルは Copilot が使用した名前や作業状況によって異なります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを追加します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![「Steer active session while Copilot is working」ダイアログが強調表示されたエージェント ページのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に追加することを確認します。

## Copilot に作業を任せる

前と同様に、更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で作業中だった手順が終わった後、新しいリクエストを処理します。

完了まで数分かかります。その間に、これまでに学んだことや確認したことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページを確認しました。Mission control では、進行中と完了済みのタスクを追跡して、作業内容をレビューし、進行中のタスクに指示を送って範囲やガイダンスを調整できます。

このレッスンでは、次のことを確認しました。

- Mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示を送り、機能の追加を依頼しました。

Copilot がアクセシビリティ機能の作業を完了したら、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## 参考資料

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
