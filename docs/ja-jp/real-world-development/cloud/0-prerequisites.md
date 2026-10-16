---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を立ち上げます。codespace では、指示ファイルを編集し、cloud agent が作成した変更をレビューします。

## ラボ用リポジトリを設定する

コードを作成するリポジトリのコピーを、[テンプレート リポジトリ][template-repository]から作成します。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。演習ではこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** を選択し、**Create a new repository** を選んで自分用のコピーを作成します。

   ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターから案内された手順に従ってください。そうでない場合は、GitHub Copilot を利用できる organization にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後の演習で参照するため、作成したリポジトリ パス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次に、Codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応したフル機能の IDE を提供します。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスをすべて手動でインストールするよりも速く完了します。その間に、次に扱う GitHub Copilot の機能を確認しておくこともできます。

> [!CAUTION]
> 後の演習で Codespace に戻ります。いまのところは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、Codespace またはローカルの [dev container][dev-containers] 内で実行する前提で作られています。どちらでも必要な前提条件がインストール済みの環境を用意できます。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

おめでとうございます。ラボ用リポジトリのコピーを作成できました。さらに、Copilot cloud agent と一緒に作業する際に使う Codespace の作成も開始しました。

## 次のステップ

次は、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## 参考資料

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
