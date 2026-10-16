---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際には、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、コードの構成方法も説明できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリ全体およびパス別の指示がエージェントに渡される方法を確認します。
- コーディング標準を守るよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys の開発チームには、開発手法に関するガイドラインと要件があります。次の内容が含まれています。

- コメントでは、コードの繰り返しではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の公開関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードを変更した場合はコメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発手法に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解できるようにコンテキストや希望を指定できます。規約、ライブラリ、コメントの書き方などを指定すれば、より適切な提案やコードを得られます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスクのためのガイドラインを記述します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式を前提とする前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには指示ファイルが用意されています。変更を加える前に、内容と影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。このフィールドでは、指示の適用対象となるファイルの glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、参考となる既存ファイルを示すことができます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として役立ちますが、まだ不足があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能のコンテキストを失わないよう、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を実行して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新済みのガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを読み込む方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリ全体およびパス別の指示がエージェントに渡される方法を確認しました。
- コーディング標準を守るよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## 参考資料

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
