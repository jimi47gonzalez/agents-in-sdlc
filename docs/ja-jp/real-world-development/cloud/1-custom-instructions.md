---
title: "演習 1 - カスタム instructions (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[instruction ファイル][instruction-files]を使うと、何をするかだけでなく、どのように行うかも Copilot に伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム instructions とパスに応じた instruction ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで並べて効果を示すのは困難です。instruction ファイルの影響は、このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに確認できます。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードを採用し、現代的な見た目にします。
- TSDoc のドキュメントコメントでコードを説明します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

instruction ファイルを使って、Copilot がこうした開発手法に沿って作業するために必要な情報を伝えます。

### カスタム instructions

カスタム instructions を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。推奨するコーディング規約やライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する instructions と、特定のファイル用の instructions を作成できます。

instruction ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の instruction ファイルです。技術スタック、開発中のものの概要、ベストプラクティスなど、多くのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。
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
> `copilot-instructions.md` の **Code formatting requirements** セクションではコーディング標準を説明していますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを追加するプロジェクト全体のルールです。以降の演習で issue を割り当てると、cloud agent がこのルールを使います。

編集の前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから instructions を読み取りますが、ここでは変更をきれいにコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Instruction の変更をコミット、プッシュ、マージする

cloud agent は issue の対象ブランチにある instruction ファイルを読み取ります。次の演習で issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が変更を使うには、instruction の更新を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で cloud agent が issue に取り組むとき、これらの instructions を読み取れるようになります。

> [!TIP]
> このワークショップでは、`main` に直接作業してもかまいません。その場合はブランチ作成を省略して、`main` に直接コミットします。ブランチを使う手順は、実際のプロジェクトでの作業方法を体験できるようにしています。

## まとめと次のステップ

このプロジェクトで Copilot が instruction ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの残りの演習で、cloud agent が作成するプルリクエストにその標準が適用される様子を確認します。

次は、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム instructions 作成のベストプラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム instructions を作成する 5 つのヒント][copilot-instructions-five-tips]
- [instruction ファイルなどのリソース集: Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
