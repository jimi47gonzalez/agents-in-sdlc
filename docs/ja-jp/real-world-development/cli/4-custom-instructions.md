---
title: "レッスン 4 - カスタム instructions で Copilot を導く"
description: "リポジトリの instructions を確認し、ドキュメント標準を追加してフィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[instruction ファイル][instruction-files]では、求めるコードの内容だけでなく、どのように構成するかも説明します。フィルター機能を実装できたので、Copilot が参照した instructions を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの instruction とパスに応じた instruction ファイルが、エージェントに適用される方法を確認します。
- コーディング標準に従うため、instructions ファイルを更新します。
- instruction ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントではコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードを変更するときにコメントも最新の状態に保ちます。
- 既存の書式と lint のガイダンスを維持します。

instruction ファイルを使って、Copilot がこれらの開発手法に沿って作業するために必要な情報を伝えます。

## Instruction ファイル

カスタム instructions を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。推奨するコーディング規約やライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する instructions と、特定のファイル用の instructions を作成できます。

instruction ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の instruction ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドを記述します。

> [!NOTE]
> ほかの instruction 形式やサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム instructions のサポート情報][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム instructions を確認する

スタータープロジェクトには、すぐ使えるように複数の instruction ファイルが用意されています。変更を加える前に、どのようなファイルがあるかを確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの instructions は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、各ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する instructions があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、instructions を適用するファイルを指定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな instruction を小さな再利用可能なファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて instructions ファイルを更新する

既存のファイルはよい出発点ですが、追加で設定する標準があります。中心となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクション (35 行目付近) を探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instructions を読み込みます。更新した instructions を適用するため、会話を終了して再開します。機能のコンテキストは維持できます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを読みます。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の内容を学習しました。

- リポジトリの instruction とパスに応じた instruction ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うため、instructions ファイルを更新しました。
- instruction ファイルがコードに与える影響を確認しました。

次は、再利用できる quality-checks skill を[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム instructions を追加する][instruction-files]
- [カスタム instructions のサポート][custom-instructions-support]
- [カスタム instructions 作成のベストプラクティス][instructions-best-practices]
- [instruction ファイルなどのリソース集: Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
