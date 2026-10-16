---
title: "レッスン 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームは自分たちの専門知識を再利用可能なエージェントとして記述できます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディングに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントがあります。

カスタム エージェントはプロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を持つ YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが含まれます。

### カスタム エージェントとエージェント スキルの比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に説明すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、スキルやほかのエージェントを調整して作業します。このワークショップのアクセシビリティ カスタム エージェントは、アクセシビリティのガイドラインに照らしてサイトをレビュー、更新します。その作業の一部として、プルリクエストのワークフローを実行するスキルや、テストを実行、管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントに唯一の正しい作成方法はありません。AI のほかの機能と同様、利用する環境やシナリオに合わせてテストし、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次のことを行います。

- カスタム エージェントの定義方法を確認します。
- カスタム エージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにすることを目指しています。最近のユーザー フィードバックでは、現在のダーク テーマはテキストと背景のコントラストが不十分で読みづらいという意見がありました。このアクセシビリティ上の課題に対応するため、デザイン チームは切り替え可能なハイ コントラスト モードを求めています。

アクセシビリティは重要であり、できるだけ早く実装する必要があります。カスタム エージェントに機能を生成してもらいます。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのようにガイドするかを確認しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` のフロントマターが必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティ対応の Web サイトを生成する際の主要な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` に定義された各カスタム エージェントが表示されます。エージェントを選択すると、その指示とツールに沿ってチャット セッションが進みます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要な箇所で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていない場合は、選択します。

   ![Chat ビューのエージェント ピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat はコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。各編集は、ファイル パスとレビュー可能な差分とともにチャット内に表示されます。
6. エージェントが変更した作業ファイルを示す **Files changed** の表示が、編集に合わせて更新されることを確認します。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなど、既存のファイルを実際に変更します。

次のレッスンでは、作業中のセッションを確認して指示を追加します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門の AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。チームの知識や標準を再利用可能なエージェントとして記述し、Copilot がより効果的に作業できるようにします。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法。

次はエージェントの作業を[監視して方向付け][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えを追加します。

## 参考資料

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [Awesome Copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視、管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
