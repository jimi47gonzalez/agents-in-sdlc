---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム instructions][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。エージェントは作業を終えると、人間の開発者がレビューできるドラフト PR を作成します。こうした委任で面倒な作業を減らし、開発プロセスを加速して、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次の内容を学習します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 適切な範囲の issue を作成する重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの最初のバージョンを作った開発者がドキュメントを残しておらず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、利用者がほかのタイトルも見つけられるようにします。現段階では完成度の高い実装ではなく、ユーザーエクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業は現在の阻害要因ですが、優先度の高い issue がほかにあります。

どちらも優先順位が下がりやすいタスクであり、Copilot cloud agent に依頼するのに適しています。cloud agent が非同期に作業している間、開発者は別のタスクに集中し、作業完了後に戻って内容をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境を設定して作業を開始します。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲で指示することの重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業にも複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、状況に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム instructions

前の演習ではカスタム instructions と、その効果を確認しました。すでに作業した場合は `.github/copilot-instructions.md` を見たことがあるでしょう。まだの場合は、cloud agent に作業を割り当てる前に、リポジトリに含まれる instruction ファイルを読み取り専用で確認してください。後で結果をレビューするときに、その影響を確認できます。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` に適用されます。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てるとき、作成された PR に TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも、これらの点を確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、適切な環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も、ソフトウェアエンジニアのように作業するための環境が必要です。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成し、作業開始前に実行できます。必要な開発ツールや依存関係を利用できるようにします。このワークショップでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを実行した SQLite データベースを利用できるよう、あらかじめ設定しています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブから手動でワークフローを実行できるため、Copilot の実行を待たずに動作をテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり不足していたりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成が得意なため、cloud agent に依頼するのに適したタスクです。バックグラウンドでドキュメントを作成し、次の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` などのタイトルが付いた新しいプルリクエスト (PR) を開きます。一覧に PR が表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、作業方法を検討しています。

13. todo リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状況を理解し、方針を検討してからコードを作成する手順が表示されます。

作業には数分かかります。Copilot cloud agent の主な目的は、作業を非同期に実行して別の作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチーム向けの関連ゲーム機能は時間のかかる作業ではないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示されます。Copilot が作業を開始したことを示しています。

![issue で作業中であることを示す Copilot の目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューすればよいので、ここからはカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当て、非同期に作業させることができます。技術的負債への対処、新機能の作成、別のフレームワークからのコード移行などに利用できます。

この演習では、次の内容を学習しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 適切な範囲の issue を作成する重要性
- issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業する間に、カスタムエージェントの作成と使用を確認します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム instructions も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
