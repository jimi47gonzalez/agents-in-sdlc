---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハーネスでは、ユーザーエクスペリエンスの改善や機能追加について、GitHub Copilot と複数の作業に取り組みました。コードのドキュメント追加、デザインチームが検討する関連ゲーム機能の実装、ハイコントラストモードとライトモードによるアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に対する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視されずにタスクを実行するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのは作業用のブランチだけです。
- cloud agent は GitHub Actions 内で動作し、作業用に分離された一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限の対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が生成した最初のプルリクエスト (PR) を確認します。これはコードへのドキュメント追加です。GitHub.com の標準的な PR インターフェイスを使って確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. ブラウザーの新しいタブでリポジトリを開きます。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付いています。その場合は Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやそのほかのドキュメントが追加されたコードを確認します。具体的な変更内容は状況によって異なります。

   TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーコメントを確認してください。これらは演習 2 の最初に確認したカスタム instruction ファイルに基づいています。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションでワークフローがキューに追加されたことを確認します。すべて問題なければ、1 つの Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同じように、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認して、対応するセッションを開始します。結果は一意ではないため、必ず依頼する文章を指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、ドキュメントへの変更を依頼するコメントを追加し、ユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は変更を行う新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、新しいコミットが PR に追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して改善できます。準備ができたら PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションでワークフローがキューに追加されたことを確認します。すべて問題なければ、1 つの Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます。この作業はカスタム instruction の更新前に割り当てたため、指示の変更は反映されていません。**Conversation** タブにコメントを追加すれば、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ向けのカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付いています。Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードを切り替える JavaScript または TypeScript コード

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションでワークフローがキューに追加されたことを確認します。すべて問題なければ、1 つの Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルを開き、Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ向けのカスタムエージェントによって、Copilot がアクセシビリティのベストプラクティスに沿って機能を実装できました。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 作業の委任を続ける

cloud agent に実際のバックログ項目を割り当てて別の作業に集中すると、cloud agent の利点を活かせます。いくつか issue を作成し、Copilot に割り当ててみましょう。次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次の内容を学習しました。

- このリポジトリのカスタム instruction ファイルを確認し、cloud agent の出力に反映されることを学びました。
- issue を Copilot cloud agent に割り当て、非同期の環境構築、計画、実行を確認しました。
- アクセシビリティ向けのカスタムエージェントを作成して使い、ハイコントラストモードとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、作業中のセッションを監視して指示を追加しました。
- cloud agent のプルリクエストをレビューして改善し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ここで学んだ内容を発展させるには、ほかのハーネスも確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skills、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次の作業を試せます。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
