---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームは自分たちの専門知識を再利用可能なエージェントとして記述できます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディングに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントがあります。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を持つ YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが含まれます。

### カスタム エージェントとエージェント スキルの比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に説明すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、スキルやほかのエージェントを調整して作業します。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビュー、更新します。その作業の一部として、プルリクエストのワークフローを実行するスキルや、テストを実行、管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントに唯一の正しい作成方法はありません。AI のほかの機能と同様、利用する環境やシナリオに合わせてテストし、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次のことを行います。

- カスタム エージェントの定義方法を確認します。
- カスタム エージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを目指しています。最近のフィードバックでは、現在のダーク テーマはテキストと背景のコントラストが不十分で読みづらいという意見がありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot をどのようにガイドするかを確認しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを持つ YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` のフロントマターが必須です。

3. 続くセクションを確認します。
   - アクセシビリティ対応の Web サイトを生成する際の主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する場所です。ここから GitHub Copilot cloud agent にタスクを割り当て、進行状況を監視し、作業中のセッションに追加の指示を送れます。まず Copilot にハイ コントラスト モードを作成するタスクを割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントとアクセシビリティ エージェントが選択された Copilot の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組みます。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは、特定のタスクや分野に対応する専門の AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントとして記述し、Copilot がより効果的に作業できるようにします。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントにタスクを割り当てる方法。

Copilot がハイ コントラスト モードを実装している間に、次は Mission control から[エージェント セッションを監視して方向付けます][next-lesson]。

## 参考資料

- [カスタム エージェントについて][custom-agents]
- [organization でカスタム エージェントを使うための準備][org-custom-agents]
- [enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視、管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
