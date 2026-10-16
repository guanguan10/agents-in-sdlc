---
title: "レッスン 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、作業用の [codespace][codespaces] を立ち上げ、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリを設定する

作成するコードには、リポジトリのコピーを使用します。[テンプレート リポジトリ][template-repository]からインスタンスを作成します。新しいリポジトリには、ラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して自分用のコピーを作成します。

   ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターから案内された手順に従ってください。そうでない場合は、GitHub Copilot にアクセスできる organization にリポジトリを作成します。

   ![リポジトリ テンプレート設定の入力画面](../../../_images/ex0-repository-settings.png)

4. 後の演習で参照するため、作成したリポジトリ パス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体でこれらの Issue を使います。自分で起票する必要はありません。

## Codespace を作成する

次に、Codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応したフル機能の IDE を提供します。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスをすべて手動でインストールするよりも速く完了します。その間に、次に扱う GitHub Copilot の機能を確認しておくこともできます。

> [!CAUTION]
> 後の演習で Codespace に戻ります。いまのところは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、Codespace またはローカルの [dev container][dev-containers] 内で実行する前提で作られています。どちらでも、必要な前提条件がインストール済みの環境を用意できます。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使用している場合は、すでにインストールされているはずです。

> [!TIP]
> GitHub Copilot Chat 拡張機能がまだインストールされていない場合は、[Visual Studio Code Marketplace][copilot-chat-extension]からインストールできます。Visual Studio Code の **Extensions** ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択する方法もあります。

拡張機能をインストールした後、GitHub アカウントで認証する必要がある場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して Enter キーを押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の手順に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

おめでとうございます。ラボ用リポジトリのコピーを作成し、コードを書き始めるときに使う Codespace の作成も開始しました。

## 次のステップ

Copilot に作業を依頼しましょう。Copilot にプロジェクトの規約を教える[レッスン 1 - カスタム指示][next-lesson]に進みます。

## 参考資料

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
