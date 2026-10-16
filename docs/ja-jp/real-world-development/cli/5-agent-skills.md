---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使用する"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、手動での動作確認や、指示ファイルを使った標準への準拠の確認が必要です。テストや lint、継続的インテグレーション (CI) のほかの要素はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、Copilot がこれらの操作を正しく実行する方法を理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは、読みやすいレポートに改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースがまとめられています。基本的には、スキル名のフォルダーと、その中の `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキルの名前と説明を定義するフロントマター、スキルの概要、スキルを呼び出す条件が記載されています。必要に応じて、フォルダーにスクリプトなどのリソースを含められます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルが含むのは `SKILL.md` のみです。プロジェクトの既存コマンドを使うためです。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、チームで共有して再利用できます。また、ユーザーのスキル フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭にある `name` と `description` を読みます。説明により、Copilot はスキルを呼び出すタイミングを判断できます。
3. 指示を読み、テストと lint の手順がどのように示されているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。スキルに 3 種類のチェックを実行してもらいましょう。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 実行後のレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールがどのような結果を報告したかがわかるように、スキルのレポートを改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを使ってみましょう。Copilot CLI では、会話を再起動しなくても編集済みのスキルを再読み込みできます。

1. 次のコマンドを実行します。

   ```plaintext
   /skills reload
   ```

2. 最初とまったく同じプロンプトを実行します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 実行後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェックに使う `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

フィルター機能と合わせて、この変更を機能 PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson] Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの次の例は参考情報であり、追加の演習ではありません。

- [Agent Skills の仕様][skill-spec]
- [貢献ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
