---
title: "演習 1 - カスタム instructions (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。コード生成のガイダンスを提供する[instruction ファイル][instruction-files]が、その代表的な方法です。

この演習では、次の内容を学習します。

- リポジトリのカスタム instructions とパスに応じた instruction ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の instructions を使って、フィルター機能の最初のデータ部分 (publishers ヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再送信し、生成コードが新しい標準に従うことを確認します。
- 次の演習で Copilot が更新したガイダンスを使えるよう、instructions とフィルター機能の変更を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確定的ではありません。この演習では出力を一字一句一致させるのではなく、instructions の更新後に動作がどのように変わるかを確認します。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードを採用し、現代的な見た目にします。
- TSDoc のドキュメントコメントでコードを説明します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

instruction ファイルを使って、Copilot がこれらの開発手法に沿って作業するために必要な情報を伝えます。

### カスタム instructions

カスタム instructions を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。推奨するコーディング規約やライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する instructions と、特定のファイル用の instructions を作成できます。

instruction ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の instruction ファイルです。技術スタック、開発中のものの概要、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に適用する instruction ファイルです。TypeScript などの言語や、UI コンポーネントの作成、ユニットテストの追加などのタスクについてガイドできます。

> [!NOTE]
> IDE では、instruction ファイルは Copilot Chat のコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの instruction と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の instruction ファイルもサポートしています。[サポートされる instruction ファイル][custom-instructions-support]を参照してください。

### Instruction ファイルを管理するためのベストプラクティス

instruction ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトにある例は、一般的な管理方法を示しています。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクトレベルの情報を記述します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関するガイドに使います。
- 自然な言葉で、明確なガイダンスを記述し、コードのよい例と避けるべき例を示します。

instruction ファイルの作り方は 1 つではありません。AI の使い方と同様に、プロジェクトに合う方法は試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な instruction ファイルを用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction ファイルを作成させることもできます。操作方法は利用する環境によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各環境のレッスンで、関連する操作を説明します。
>
> テンプレートや出発点を探している場合は、instruction ファイル、カスタムエージェントなどのリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム instructions を確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の複数の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスもあります。これらは Copilot とのあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリルートを基準とする glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. プロジェクトのユニットテスト作成に関する instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの別の instruction ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな instruction を小さな再利用可能なファイルに分け、コード生成の参考例を Copilot に示せます。ここにあるパスはリポジトリルートではなく、instruction ファイルを基準としています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントの規則を追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instructions の更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

現在の instruction ファイルでプロンプトを送信し、カスタム instructions の効果を確認します。後でファイルを更新し、同じプロンプトをもう一度送信して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストが変更されることもあります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案コードには、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。この演習では、instruction の更新後に動作の一貫性が高まることを確認します。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に伝えるためのファイルです。コーディング標準にルールを追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準を説明していますが、コード内のドキュメントについてのルールがありません。
3. 既存の標準の直後に、ファイルヘッダーコメントと TSDoc のドキュメントコメントを追加する次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction ファイルはリポジトリレベルの `.github/copilot-instructions.md` に置いて全体に適用することも、特定の言語、ファイルの種類、タスク用の `*.instructions.md` として作成することもできます。ドキュメントコメントのルールのように、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再送信して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot をガイドして新しいプロジェクト標準を適用し、次の演習で活用する実際のコードに反映できました。

## Instructions とブランチをコミットする

instruction ファイルも、ほかのリポジトリアセットと同様にソース管理で管理します。instructions とブランチをコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで、instructions とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. instructions と、ヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。Copilot に issue を探させて PR を作成し、issue をクローズするようにしてからマージします。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートには GitHub のツールがあらかじめ設定されているため、追加のセットアップは不要です。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従ってください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. instructions の更新と新しい publishers ヘルパーの両方を説明する PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドの実行許可を求めたら、内容を確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot が instruction ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の instructions でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再送信し、提案コードが新しい標準に従うことを確認しました。
- instructions とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に機能を追加するときに、[agent mode でこれらの instructions を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム instructions 作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム instructions を作成する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム instructions][personal-instructions]
- [instruction ファイルなどのリソース集: Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
