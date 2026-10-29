---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことは、コードを記述するだけではありません。データや外部サービスを扱い、Copilot が使える自動化も追加します。そのために MCP サーバーを使います。MCP サーバーを使うと、Copilot CLI に組み込まれている機能に加えて、ツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に操作する必要があります。ユーザーと同じように Copilot が Web サイトを使えるようにすると、変更が想定どおりに動作する確信を高め、実装プロセスをさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP を利用すると、AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、最新の情報にアクセスしたり、ユーザーに代わって操作したりできます。

MCP サーバーは、AI エージェントと外部ツールやサービスをつなぐ橋渡し役です。それぞれの MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> キーを押します。
4. <kbd>Enter</kbd> キーを押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> キーを押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> キーを押して MCP ダイアログを閉じます。

## Playwright で機能を調べるよう Copilot に依頼する

前のレッスンでは、機能が想定どおりに動作することを手動で確認しました。今度は、Copilot が利用できる Playwright MCP サーバーで同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーの利用方法を判断するため、MCP サーバーを使うよう指示する必要はありません。ただし、使用するツールが分かっている場合は、明示するとより一貫した結果が得られ、トークンも節約できます。

2. Copilot がブラウザー上で機能を確認するために実行している手順を確認します。
3. レポートを読み、すべて想定どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、レポートを返します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を調べました。このレッスンでは、次の内容を学習しました。

- Model Context Protocol (MCP) と Copilot CLI での使い方
- まだ利用できない場合に Playwright MCP サーバーを追加する方法
- エージェントにブラウザーを操作させてフィルター機能を調べる方法

次は、skill とブラウザーツールを専門的な役割にまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
