---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チーム向けの関連ゲーム機能を作り、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックして改善してもらいましょう。

### シナリオ

これまでに何度も確認したように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は、監督者がいない状態で非同期にタスクを実行するため、安全性を保つためのセキュリティ上の制限があります。

- Copilot はリポジトリに対する読み取りアクセスと、自身が使うブランチに対する書き込みアクセスのみを持ちます。
- Cloud agent は GitHub Actions 内で動作し、作業用に個別の一時的な環境を作成します。
- GitHub Actions ワークフローは、実行前に人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限][agent-firewall]されています。MCP サーバーもその対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードへのドキュメント追加 PR を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は作業が終わるまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![「Files changed」タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを確認します。具体的な変更内容は状況によって異なります。

   TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認してください。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。
6. 変更をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの実行に承認が必要であることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![「Approve and run workflows」](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローがキューに入ります。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は一方通行ではありません。チームのメンバーと同様に、プルリクエストやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は確率的に変わるため、依頼する文面を固定することはできません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考にして、生成されたドキュメントの変更を依頼するコメントを追加します。ほかのユーザーと同じように **@copilot** をメンションしてください。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![「Back to pull request」](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、新しいコミットがプルリクエストに表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足できるまで反復して作業を依頼できます。問題がなければ、ドラフト PR をレビュー準備完了に変更し、`main` ブランチにマージできます。

![レビュー準備完了に変更する PR](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 変更をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローの実行に承認が必要であることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![「Approve and run workflows」](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションにワークフローがキューに入ります。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) を `<branch-name>` に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。前と同様に Copilot cloud agent に追加の変更を依頼し、反復して作業できます。表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc のドキュメント コメントを追加したりできます。この Issue は、カスタム指示を更新する**前**に割り当てたことを思い出してください。前と同じように、**Conversation** タブで新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で割り当てたハイ コントラスト モードと、演習 4 の Mission control から追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は、作業が終わるまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を維持するためのローカル ストレージの使用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. 変更をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの実行に承認が必要であることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![「Approve and run workflows」](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローがキューに入ります。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリケーションを起動し、ハイ コントラストとライト モードの切り替えをブラウザーで試します。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントによって、アクセシビリティのベスト プラクティスに沿った機能を Copilot が実装できました。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent にバックログの実際の作業を任せ、その間に別の作業を進めると効果的です。リポジトリにいくつかの Issue を追加して Copilot に割り当て、その習慣を身に付けましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力への影響を調べられるようにしました。
- **Issue を Copilot cloud agent に割り当て**、環境をセットアップして、計画し、非同期に実行する様子を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを Mission control として使い、アクセシビリティ セッションを監視して途中で方向付けました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼して、ワークフローの実行を承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学習する場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューして公開します。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト範囲の拡充に関する Issue を cloud agent に割り当てます。
- 別の分野向けのカスタム エージェントを作成します。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定します。

## 参考資料

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
