---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが 1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** に作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質のワークフローを 1 つの会話とブランチで進め、すべての変更をこの PR に含めます。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージまでの流れをどのように自動化するかを確認します。
- 機能全体の変更と検証の証拠を調べます。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされることを確認します。

## シナリオ

フィルター機能の開発を通して、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys では、開発者がマージの承認権限を保ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot が PR を妨げている項目を処理します。継続的インテグレーション (CI) の失敗を修正し、レビュー コメントに対応し、必要に応じてリベースした後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、ユーザーが **Merge pull request** を選択していました。Agent Merge にこの作業を任せることもできますが、有効にする前に変更をレビューして、マージの準備ができていると判断してください。

## Agent Merge で PR を管理する

コードが揃ったので、全体をレビューして PR を作成し、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を実行してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` を実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge が、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避することはありません。ブロックされた場合は理由を確認し、リポジトリで許可されている範囲で、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。次のことを行いました。

- Agent Merge とは何か、マージまでの流れをどのように自動化するかを確認しました。
- 機能全体の変更と検証の証拠を調べました。
- フィルター機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[便利な Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## 参考資料

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
