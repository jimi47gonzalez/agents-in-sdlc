---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの小さな更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグして、変更漏れがあればさらに修正します。作業は次々と続きます。

このようなときに Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者と同様に、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、問題が見つかると自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。Agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームのペアプログラマーとして非同期に作業し、issue やプルリクエストを通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法
- GitHub Copilot Agent Mode を使って UI とデータ層にまたがる機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode を使って、残りのデータ層、UI、テストを実装します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者向けテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。サイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接参照します。個別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から起動します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace で実行している Web サーバーへのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、次の機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認します。テンプレートからプロジェクトを設定すると、issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** はファイルをまたぐ変更を自律的に計画・実装し、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。コードの提案に一貫性を持たせ、よい体験を提供することが目的です。ただし、大規模言語モデル (LLM) は確率的に動作するため、ワークショップと異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考える順に issue を並べます。

## Instruction ファイルを確認する

コードの生成を依頼する前に、Copilot にコンテキストを伝える instruction ファイルを確認します。Web サイトの機能追加のアプローチを説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などの各ファイルに関する instruction へのリンク

> [!TIP]
> instruction ファイルから、ほかの instruction ファイルやプロジェクト内のファイルを参照できます。パスは instruction ファイルの場所を基準とします。これにより、再利用や複雑な instruction の分割が容易になり、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ層 (`src/lib/`) に残りのフィルターロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページを更新し、フィルター UI を追加します。

また、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instruction ファイルを指定します。

   ![UI instruction ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルに引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを探す様子を確認します。データ層ヘルパー、UI、テストを見つけた後、ファイルの変更とテストを始めます。

   ![Copilot がプロジェクトファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行うため、タスクの複雑さやコードベースの規模によって数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了すると、すべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot の作業中に一時停止したり、一部のテストが失敗したりする場合があります。問題ありません。タスクが完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のセッションが完了した Chat 画面のスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode で Tailspin Toys に機能を追加しました。次の内容を学習しました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する方法
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページングを実装する

ゲーム一覧が増えると、ページングが必要になります。この演習で学んだことを使い、Copilot にページングを追加するよう依頼します。次の点を検討してください。

- 既存の instruction ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーに選ばせるか、固定にするかなど、ページングの方式を検討します。
- 希望する実装に必要なガイダンスを、プロンプトに含めます。
- Copilot に変更を依頼してコンテキストを追加しながら反復する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: 各モードの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
