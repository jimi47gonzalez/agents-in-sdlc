---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザーエクスペリエンスの改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまな方法で管理できます。この mission control では、プルリクエスト (PR) が開いているすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次の内容を学習します。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加して機能を依頼します。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能の実装が進んでいるため、ハイコントラストに加えてライトモードも作るようにエージェントへ指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されるタスクの一覧を確認します。たとえば、次のタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が行った作業を確認します。次のような作業が表示されます。
   - リポジトリをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーを設定する
   - 割り当てられたタスクを完了するための作業を行う

> [!NOTE]
> 表示される手順は、Copilot の進行状況や作業方法によって異なります。

4. 右側に表示される PR ペインも確認します。PR や変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に指示を追加する

現在実行中のタスクを確認したので、ハイコントラストモードの作業にライトモードの切り替えも含めるよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control に表示されるアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents ページの cloud agent タスク。作業中のセッションに指示を追加するダイアログが強調表示されています。](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> キーを押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に反映することを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼をワークフローに組み込みます。

前の演習と同様に完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して指示するための中心的な場所である Copilot agents ページを確認しました。mission control では、進行中および完了したタスクを追跡し、実行中の作業をレビューし、進行中のタスクに追加の指示を送れます。

この演習では、次の内容を学習しました。

- mission control とエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに指示を追加し、機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
