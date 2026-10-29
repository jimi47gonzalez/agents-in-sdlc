---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、エージェント型コーディングアシスタントとして GitHub Copilot をターミナルから利用できます。コードベースの調査、コードの生成、コマンドの実行、外部ツールへの接続をコマンドラインで行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の作業を一連の流れで進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。
2. 星評価の変更を行い、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルターの issue を起点に、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの instructions を更新し、フィルターの作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. quality assurance (QA) カスタムエージェントを作成して使い、要件、テスト範囲、検証結果をレビューします。
8. フィルターの変更全体をレビューし、Agent Merge を使って PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウド委任に役立つスラッシュコマンドを試します。

ワークショップでは、星評価とフィルターの 2 つの PR を作成します。フィルターの PR には instructions、skill、QA プロファイル、テストの更新も含めます。フィルターと品質改善の作業は、同じ会話とブランチで進めるため、各ツールを学びながら変更を積み重ねられます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認します |
| [2. 星評価を追加する: 手早く成果を出す][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue を起点に機能を計画し、Autopilot で実装して Interactive モードでレビューします |
| [4. カスタム instructions で Copilot を導く][ex4] | コンテキスト | instructions を確認、更新し、フィルター機能に適用します |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存 skill を確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルターの動作を調べます |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門エージェントを作成して選択し、最終検証の結果を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使います |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウドエージェントへの任意の委任を試します |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、追加リソースを確認します |
| [任意: Foundry を取り入れる][foundry] | ホステッドエージェント | モデルを準備し、カタログに基づくエージェントをデプロイして、Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
