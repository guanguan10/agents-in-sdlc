---
title: "レッスン 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際には、コンテキストが重要です。タスクの実行方法が決まっている場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのが、コード生成の指針を伝える[指示ファイル][instruction-files]です。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を適用した状態で、フィルター機能の最初のデータ処理 (`publishers` ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再度実行し、生成コードが新しい標準に沿う様子を確認します。
- 指示の変更とフィルター処理の最初の部分を `main` にコミットし、次のレッスンで Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、設定した標準に沿わないことがあります。Copilot は確率的に動作します。このレッスンでは、出力を一字一句一致させるのではなく、指示を更新した後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys の開発チームには、開発手法に関するガイドラインと要件があります。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの手法に沿って作業するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解できるよう、コンテキストや希望を指定できます。規約、ライブラリ、コメントの書き方などを指定すれば、より適切な提案やコードを得られます。リポジトリ全体に適用する指示も、特定のタスクやファイル形式に適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot へのほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記述します。使用する技術スタック、開発対象の概要、ベスト プラクティス、全体に適用するガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル形式のために作成できます。TypeScript や Astro などの特定の言語、UI コンポーネントや新しいユニット テストの作成などのタスクに関するガイドラインを記述します。

> [!NOTE]
> IDE を使う場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく扱うことは、このワークショップの範囲外です。サンプル プロジェクトにあるファイルは、一般的な方法の一例です。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体のコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、ファイル形式 (ユニット テスト、Astro コンポーネント、データ層など) や特定のタスクのための指示を記述します。
- 自然な言葉を使い、明確なガイダンスを記述します。コードの望ましい例と避けるべき例を示します。

AI の使い方が 1 つではないのと同様、指示ファイルの作成方法にも決まった正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

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

このリポジトリには、コアの `copilot-instructions.md` と、各種タスクの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI でファイルを確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの指示があることを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドはリポジトリのルートを基準とする glob を設定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分けたり、Copilot が従う例を示したりできます。ここでのパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc コメントとファイル ヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前のレッスンで使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要な箇所で説明します。

カスタム指示の効果を確認するため、最初に現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント ピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを適用します。多くの場合、ヘルパー ファイルとテストの両方を変更します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、publisher の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに、TSDoc コメントとファイル レベルのコメント ヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくてもコメントが追加される場合があります。その場合も問題ありません。指示を更新した後に動作が一貫して改善することを確認するのが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善できるよう、リポジトリのコーディング標準を記述します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内のドキュメントに関する規則はないため、生成されたヘルパーにドキュメント コメントがありません。
3. 既存の標準の直下に、ファイル ヘッダーと TSDoc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、`*.instructions.md` として言語、ファイル形式、タスクごとに作成することもできます。ここで追加したドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次のレッスンで使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリのほかのファイルと同様に、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎部分のために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、プルリクエストを作成し、バックログにあるドキュメント標準の Issue に関連付けます。手動でもできますが、プロジェクトに接続済みの GitHub ツールを使って、Issue を探し、Issue を完了する PR を作成し、マージするよう Copilot に依頼します。

> [!NOTE]
> Copilot が GitHub に対して操作する最初のレッスンです。プロジェクトのテンプレートでは GitHub ツールがワークスペースに接続済みなので、追加のセットアップは必要ありません。初めて Copilot がツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。プロンプトに従って許可します。後のレッスンで、この接続に使う Model Context Protocol について学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する Issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と publisher ヘルパーの基礎部分の両方を説明に含めます。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に代わって GitHub 操作を実行するための **Allow** が表示されたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、PR をマージしてローカルのブランチを `main` に戻すよう、次のプロンプトで依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に代わって GitHub 操作やシェル コマンドを実行するための **Allow** が表示されたら、コマンドを確認して適切な場合に選択します。

GitHub Copilot の支援を受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に沿う様子を確認しました。
- 次の演習に備えて指示とヘルパーの基礎部分をコミットし、`main` に反映しました。

次は[エージェント モード][next-lesson]を使い、Copilot にコードベース全体にまたがる新機能を追加してもらいます。

## 参考資料

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
