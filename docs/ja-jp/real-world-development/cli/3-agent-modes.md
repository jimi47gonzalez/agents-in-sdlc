---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。より大きな変更には、しっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作し、必要なものを正しい方法で構築できるようにします。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを体験します。issue を使って機能を実装し、コードが有効で、機能が想定どおりに動作することを確認し、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中するため、手順を簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub issue から Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- Codespaces のポート転送を使ってブラウザーで機能を手動で検証します。

機能の作業を続けながら、リポジトリの instructions を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリや発行元で絞り込めるようにしたいと考えています。バックログの issue には機能の概要がありますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan モードで決定事項を整理し、範囲を明確にしてから Autopilot に実装を依頼します。

## 背景

AI コーディングエージェントを開発ワークフローに加えても、基本的な手順は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明する issue から始めます。
2. 構築するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行し、コードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、ほとんどの開発プロセスは上記の流れに沿っています。

この標準的な手順に従うことで、AI が生成したコードが要件を満たし、手動で書いたコードと同じレビューと検証を受けるようにできます。

## 会話モード

**会話モード**は、エージェントがどれだけ自律的に作業できるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的に作業し、コードを書いてテストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot モードで実装します。

## issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、アプローチを定義できます。複雑な作業では、時間をかけて計画を立てましょう。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、最適なアプローチを検討し、計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に合わせて回答します。選択に正解や不正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 決めた内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたので、実装に進みます。Autopilot を使って Copilot に作業を依頼します。

Autopilot では、Copilot がタスクを完了したと判断するまで作業を繰り返します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って作業を進め、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を issue と計画時に決めた内容と照らし合わせます。
4. コードを確認したら <kbd>Esc</kbd> キーを押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードは問題なさそうですが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. カテゴリによる絞り込み、発行元による絞り込み、計画で決めた組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を構築してレビューしました。このレッスンでは、次の内容を学習しました。

- GitHub issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- Codespaces のポート転送を使ってブラウザーで機能を手動で検証しました。

次は、コード生成の動作をさらに詳しく確認します。Copilot が文書化されたプラクティスに従うよう、[カスタム instructions を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
