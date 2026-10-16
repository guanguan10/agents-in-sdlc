---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデルの選択、セッション共有、任意のクラウド委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの優れた CLI ツールと同様に、GitHub Copilot CLI には操作用のスラッシュ コマンドが数多く用意されています。高度な機能、内部の情報、追加の設定項目を利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使用しました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を確認します。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

規模の大きなタスクや複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的にコンパクトにしますが、スラッシュ コマンドを使って自分で確認したり圧縮したりすることもできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを実行します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域ごとのコンテキストの内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行し、結果を比較します。会話が短い場合、大きな違いはないことがあります。

> [!NOTE]
> Copilot CLI は、コンテキスト ウィンドウがいっぱいになると自動的に内容を圧縮します。圧縮するタイミングを自分で決める場合は `/compact` を使います。無関係な別のタスクに移る場合は、`/clear` または `/new` で新しい会話を始めます。

## モデルを選択する

モデルによって得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを実行します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、得た知識を共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を実行し、インストールされているバージョンでの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に適した方法を選びます。たとえば `/share file` ではローカルの Markdown ファイルにエクスポートできます。
3. エクスポートした内容を送信または公開する前にレビューします。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並列化または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、規模の大きなタスクに使える次のコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分け、並行して実行できます。
- `/worktree` は、別のタスクのために独立した Git worktree を作成できます。
- `/delegate` は、非同期で動作してプルリクエストを開くことがある Copilot cloud agent にタスクを委任できます。

これらのコマンドを使うと、worktree やリモート作業が追加される可能性があるため、任意の操作としています。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく知りたい場合は、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更し、セッションを共有して、内部の状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を確認しました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。最後に[これまでの学習内容を振り返り][next-lesson]、今後の学習に役立つ情報を確認しましょう。

## 参考資料

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
