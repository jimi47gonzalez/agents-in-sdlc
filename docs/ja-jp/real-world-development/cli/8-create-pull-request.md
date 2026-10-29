---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、instructions、skill の更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、instruction の更新、skill の更新、品質保証 (QA) プロファイル、テストを 1 つのブランチに保存しました。変更全体をレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** に作業を任せます。

> [!NOTE]
> 通常は、機能、instructions、skill の更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質改善の作業を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージまでのワークフローを自動化する方法を学びます。
- 機能の変更全体と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージを許可するかどうかは開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に対処します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなどを行い、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に変更をレビューし、マージしてよい状態かどうかを判断してください。

## Agent Merge で PR を管理する

コードの作成が完了したので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> キーを押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、必要な権限の不足を回避しません。作業がブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの PR を手動でマージします。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発の複数の手順を自動化しました。このレッスンでは、次の内容を学習しました。

- Agent Merge と、マージまでのワークフローを自動化する方法を学びました。
- 機能の変更全体と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[便利な Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
