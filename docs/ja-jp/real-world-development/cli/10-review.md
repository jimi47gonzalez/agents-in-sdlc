---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つの PR、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。実施した内容は次のとおりです。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター issue を見つけました。
- 星評価を追加し、Codespaces のポート転送を使ってブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの issue を起点に、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム instructions でエージェントをガイドし、既存の `quality-checks` skill をカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、skill の結果、ブラウザーでの確認結果を評価しました。
- フィルター機能の変更全体をレビューし、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR をマージします。

1. **星評価:** ゲームカードに既存の `starRating` と未評価状態を明示して表示します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、instructions を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して関連するテストを含めます。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを円滑に進めるため、この作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー調査など、複数の方法でコードを確認しました。`quality-checks` skill はユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。PR の前に、QA でこれらの結果と要件およびテストカバレッジのレビューをまとめました。

追加するテストは実際の不足を補うものにします。新しいテストが不要な QA 実行も正しい結果になり得ます。マージを許可する前にコードと検証結果をレビューし、変更後は影響する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、その作業を左右します。このワークショップでは、instructions を更新し、skill をカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整してください。instructions は標準を定め、skill は繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合った **モードとモデル** を選びます。構築前にアプローチを検討するには **Plan**、対象を絞った変更で作業に関与し続けるには **Interactive**、範囲の明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高い推論能力を持つモデルを選びます。

コンテキストは、基盤と同じくらい重要です。何を、なぜ、どのように構築するかを明確に説明すると、出力が大きく変わります。

## さらに確認する機能

コアワークフローを学びました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- 変更を分析するコードレビューエージェントに依頼する `/review`
- 問題について考え、別の視点を得る `/rubber-duck`
- 独立したサブタスクを並行して調整する `/fleet`
- 別のタスクを分離する `/worktree`
- Copilot cloud agent にタスクを送信する `/delegate`

## 次のステップ

どのツールも、使い続けることが上達への近道です。実稼働コード、趣味のコード、長年構想していながら作る機会がなかった小さなアプリなどで活用してください。学んだことをチームと共有し、チームからも学びましょう。引き続きドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張するには、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。別の作業環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
