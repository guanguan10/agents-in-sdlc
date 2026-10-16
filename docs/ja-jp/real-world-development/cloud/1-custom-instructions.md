---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際には、コンテキストが重要です。タスクの実行方法や Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、このレッスンでは変更前後のプロンプトを使いません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムで並べて影響を示すことが難しいからです。後のレッスンで cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys の開発チームには、開発手法に関するガイドラインと要件があります。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの手法に沿って作業するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解できるよう、コンテキストや希望を指定できます。規約、ライブラリ、コメントの書き方などを指定すれば、より適切な提案やコードを得られます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot へのほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記述します。使用する技術スタック、開発対象の概要、ベスト プラクティス、全体に適用するガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル形式のために作成できます。TypeScript や Astro などの特定の言語、UI コンポーネントや新しいユニット テストの作成などのタスクに関するガイドラインを記述します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく扱うことは、このワークショップの範囲外です。サンプル プロジェクトにあるファイルは、一般的な方法の一例です。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体のコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、ファイル形式 (ユニット テスト、Astro コンポーネント、データ層など) や特定のタスクのための指示を記述します。
- 自然な言葉を使い、明確なガイダンスを記述します。コードの望ましい例と避けるべき例を示します。

AI の使い方が 1 つではないのと同様、指示ファイルの作成方法にも決まった正解はありません。プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトで、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新用][ui-instructions]や [Astro 用][astro-instructions]など、さまざまなタスクの指示があります。
>
> Copilot に指示ファイルを生成してもらうこともできます。操作は利用する環境によって異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当する環境のレッスンで説明します。
>
> テンプレートやひな形を探す場合は、指示ファイル、カスタム エージェントなどが集められた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリにある指示ファイルを読みます。コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドはリポジトリのルートを基準とする glob を設定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分割し、Copilot が従う例を示すことができます。ここでのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ要件に含まれていません。次の手順で、TSDoc コメントとファイル ヘッダーの規則を追加します。

## 新しいリポジトリ標準を追加する

ここで編集するのは、コード内に TSDoc コメントとファイル レベルのコメント ヘッダーを記述するというプロジェクト全体の規則です。次の演習で Issue を割り当てると、cloud agent がこの規則に従います。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは変更をきれいにコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コード提案を改善できるよう、リポジトリのコーディング標準を記録します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。標準は記載されていますが、コード内のドキュメントに関する規則はまだないため、生成されたヘルパーにドキュメント コメントがありません。
3. 既存の標準の直下に、ファイル ヘッダーと TSDoc のドキュメント コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、`*.instructions.md` として言語、ファイル形式、タスクごとに作成することもできます。ここで追加したドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュしてマージする

Cloud agent は、Issue が対象としているブランチから指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチを作成します。そのため、cloud agent が指示を取得できるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して `main` に直接コミットできます。実際のプロジェクトでの作業方法に近づけるため、ここではブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。残りのハーネスで cloud agent が生成するプルリクエストに、この標準が反映されます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## 参考資料

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
