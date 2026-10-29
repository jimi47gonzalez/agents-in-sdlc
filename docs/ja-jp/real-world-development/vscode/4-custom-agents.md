---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業を効果的に行えるよう、焦点を絞った instructions、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に伝えられます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag]への準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、instructions を定義する Markdown プロンプトを記述します。

### カスタムエージェントと agent skills の違い

カスタムエージェントと[agent skills][agent-skills-concept]には一部共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。違いを簡潔に表すと、**カスタムエージェントは作業者、skill はツール**です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一部として skill や別のエージェントを指揮できます。この演習では、アクセシビリティ向けのカスタムエージェントがサイトを確認して更新します。その作業で、プルリクエストのワークフローを実行する skill やテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試しながら改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく作成します。

この演習では、次の内容を学習します。

- 既存のアクセシビリティ向けカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ向けカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントはすでに用意されています。内容を確認して、Copilot をどのようにガイドするかを理解しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルター PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成するときの主な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` に定義したすべてのカスタムエージェントが表示されます。エージェントを選択すると、その instructions とツールを使う会話を開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. まだ選択されていない場合は、Chat ビューのエージェントドロップダウンから **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントのドロップダウンを選択します。ここには現在のエージェントが表示され、既定では **default** が選択されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案してプロジェクトファイルに適用します。変更ごとに、ファイルパスとレビューできる差分がチャットに表示されます。
6. 変更が適用されると、エージェントが更新したファイルの一覧を示す **Files changed** の内容も更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を加えます。既存の Astro コンポーネント、CSS、関連するテストなどを編集します。

次の演習では、この作業の進行中に内容をレビューして追加の指示を送ります。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用できる形にまとめ、Copilot が作業を効果的に進めるためのガイドを提供できます。

この演習では、次の内容を学習しました。

- カスタムエージェントの定義方法
- Copilot Chat の agent mode でカスタムエージェントを使う方法

次は、エージェントの作業を[監視して指示を追加し][next-lesson]、変更を確認しながら、同じセッションでライトモードの切り替えを依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
