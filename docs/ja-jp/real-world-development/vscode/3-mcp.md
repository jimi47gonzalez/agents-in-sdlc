---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーを操作させ、フィルター機能を手動でテストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にアクセスできると、さらに便利になります。Copilot がそのために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![Agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントはリソースから最新の情報にアクセスしたり、ツールを使ってユーザーに代わり操作したりできます。

MCP サーバーは、AI エージェントと外部のツールやサービスをつなぎ、通信を管理します。既存の API や npm パッケージなどのローカルツールにも接続できます。各 MCP サーバーは、エージェントがアクセスできるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、リポジトリの作成や更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI エージェントは、Web ページの移動、フォームの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースにアクセスできる MCP サーバーがほかにもあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティを考慮し、MCP サーバーはほかのプロジェクト依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス権を与える場合は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがあらかじめ設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` 設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、GitHub ツールを Copilot が初めて使うときに認証を求めます。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能をテストできます。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、Codespace には表示用デスクトップがないため、Playwright がブラウザーのウィンドウを表示せずに実行するように指定します。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストで Playwright を使うため、ブラウザーはすでにインストールされていることがほとんどです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか確認された場合は、構成を確認し、サーバーを起動できるように信頼します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときに VS Code が起動し、信頼するか尋ねます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分で選択して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み上げます。会話を離れずに機能の動作をすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を参照します。つまり、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を読み取ります。簡単な機能確認を、アクセシビリティの基本確認にも活用できます。

MCP サーバーに接続し、アプリを起動した状態で、作成したフィルター機能を確認するよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を返します。レポートを issue の受け入れ条件と照らし合わせます。動作に問題がある場合は、追加の質問をするか、PR を作成する前にコード修正を依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときは、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認できたので、チームでレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、複数の方法で PR を作成できます。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する issue を探し、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるように、エージェントドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を集めたり操作したりできるようにします。GitHub ツールを初めて使うときは GitHub への認証を求められることがあります。画面の指示に従います。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が作成したリンクから PR を開いてレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の内容を学習しました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組み
- Playwright MCP サーバーをワークスペースに追加する方法
- Copilot にブラウザーを操作させ、公開前にフィルター機能をテストする方法
- ブランチを公開し、フィルター機能のプルリクエストを作成する方法

機能の**テスト**に MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
