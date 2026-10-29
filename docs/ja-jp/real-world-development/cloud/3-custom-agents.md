---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。プロジェクトの `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルでエージェントを定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、手順を定義する Markdown プロンプトを記述します。

チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag]への準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、テストパターンを管理するテストエージェントなどです。

### カスタムエージェントと agent skills の違い

カスタムエージェントと[agent skills][agent-skills-concept]には一部共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を指示します。違いを簡潔に表すと、**カスタムエージェントは作業者、skill はツール**です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一部として skill や別のエージェントを呼び出して指揮できます。この演習では、アクセシビリティ向けのカスタムエージェントがサイトを確認して更新します。その作業で、プルリクエストのワークフローを実行する skill やテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試しながら改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを学びます。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスク割り当て

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、ダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

## アクセシビリティ向けカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントはすでに用意されています。内容を確認して、Copilot をどのようにガイドするかを理解しましょう。

Codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成するときの主な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当てたり、進行中の作業を監視したり、追加の指示を送ったりできます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てのダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![クラウドエージェントの割り当て画面。カスタムエージェントの一覧で Accessibility が強調表示されています。](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用できる形にまとめ、Copilot が作業を効果的に進めるためのガイドを提供できます。

この演習では、次の内容を学習しました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスク割り当て

Copilot がハイコントラストモードの実装を進めている間に、[mission control からエージェントのセッションを監視して指示を追加します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
