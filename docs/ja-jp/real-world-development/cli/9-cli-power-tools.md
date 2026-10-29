---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使うスラッシュコマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI のコアワークフローを完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能をいくつか確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って内容を確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム instructions、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、変化があまり見られないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関係のない別のタスクを始める場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルによって得意な作業は異なり、開発者ごとに好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> キーを押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルに Markdown を出力する `/share file` など、用途に合った出力先を選択します。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を並行化または委任する

ワークショップの主要な部分は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割して並行実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期に作業し、プルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意の内容です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部で起きていることを確認したりできます。このレッスンでは、次の内容を学習しました。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにもスラッシュコマンドがあり、さらに試すことができます。最後に、[学んだことと次のステップを振り返りましょう][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI でのコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
