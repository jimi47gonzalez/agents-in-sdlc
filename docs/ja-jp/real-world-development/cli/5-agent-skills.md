---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを記述するだけではありません。手動でコードの動作を確認し、instruction ファイルを使って標準に従うようにしました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には **agent skill** が適しています。skill を使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の内容を学習します。

- 既存の `quality-checks` skill を確認します。
- 結果の形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## Instructions、スクリプト、リソース

Agent skill には、再利用できるタスクの手順、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースをまとめられます。skill の基本形は、skill の名前が付いたフォルダーと、その中の `SKILL.md` という Markdown ファイルです。Markdown ファイルには、skill の名前と説明を定義する frontmatter、処理の概要、呼び出すタイミングのガイダンスを記述します。必要に応じて、フォルダーにスクリプトやほかのリソースを含めることもできます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

skill は、チームで共有して再利用するリポジトリアセットとしてプロジェクトの `.github/skills` フォルダーに置くことも、ユーザーの skill フォルダー `~/.copilot/skills` に置くこともできます。

## skill を確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に使う `quality-checks` skill を確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は、Copilot が skill を呼び出すタイミングを判断するのに役立ちます。
3. 手順を読み、テストと lint のプロセスをどのように案内しているか確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot に skill の 3 つのチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、その成否、ツールが報告した内容を確認できるレポートに改善しましょう。skill を更新して、その形式でレポートを出力するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更した skill を、会話を再起動せずに Copilot CLI で再読み込みして実行します。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最初のレポートと比較し、結果を確認します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次の内容を学習しました。

- 単体テスト、lint、型チェックに使う `quality-checks` skill を確認しました。
- 結果の形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、フィルター機能の PR に含めます。次に、[Playwright MCP サーバー][next-lesson]を使って Copilot がサイトを直接操作できるようにします。

## skill の例

コミュニティの次の例は参考資料であり、このレッスンで追加の作業は必要ありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
