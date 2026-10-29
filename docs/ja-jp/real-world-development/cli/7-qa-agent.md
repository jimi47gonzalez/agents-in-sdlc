---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーからフィルターの動作を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが instructions、skill、MCP ツールと連携する方法を確認します。
- 再利用できる品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の issue に基づく結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルでは、エージェントの目的、instructions、利用できるツールを指定します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instructions はチーム標準を説明します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告する方法を Copilot に伝えます。既存の機能を置き換えたり、別の会話を始めたりする必要はありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用できる QA プロファイルの作成を Copilot に依頼します。プロファイルでは、QA で実行するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

作成したエージェントを使う前に、意図した QA ワークフローと境界がプロファイルに設定されていることを確認します。確認に限った作業を依頼しているのに、エージェントが機能を変更する事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、設定するとエージェントを識別しやすくなります。
3. プロファイルの instructions を読み、QA が要件から始め、リポジトリの instructions に従い、`quality-checks` skill を実行し、Playwright MCP を使うことを確認します。
4. QA が根拠となる情報を報告し、実装コードを変更する前に確認を求め、コミットやプルリクエストの作成を行わないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。そうすると、会話にある issue と計画の決定事項を利用できます。

1. 次のコマンドを入力してエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、利用可能なエージェントの一覧にまだ表示されないことがあります。エージェントは作成されているため、上記のコマンドで有効にできます。

2. 次のプロンプトを送り、QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、エージェントが返したレポートを読みます。

## まとめと次のステップ

再利用できる専門的な役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次の内容を学習しました。

- カスタムエージェントが instructions、skill、MCP ツールと連携する方法を確認しました。
- 再利用できる品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の issue に基づく結果をレビューしました。

これで、実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[変更を機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
