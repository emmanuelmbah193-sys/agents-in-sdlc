---
title: "レッスン 4 - カスタム指示による Copilot のガイド"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] には、求めるコードだけでなく、その構造や形式も記述できます。フィルター機能を実装したところで、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスごとの指示が、どのようにエージェントに適用されるかを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントにはコードを言い換えるのではなく、意図や分かりにくい判断を記述します。
- `db/` と `src/lib/` にあるエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、注入可能な `db` 引数も文書化します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更されたときにはコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための適切な情報を Copilot に渡します。

## Instruction file

Custom instruction は、コンテキストや要望を Copilot に渡し、コーディングスタイルや要件を理解しやすくします。好みの規約、ライブラリ、コメントの書き方などを指定でき、提案やコードスニペットをより適切なものにできます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを提供します。

> [!NOTE]
> ほかの instruction 形式やサポート状況は、作業環境によって異なります。特定の形式を使用する前に、[custom instructions のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでに instruction file が含まれています。変更を加える前に、ファイルを確認して既存の内容を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。ターミナルではなく、エディターで開いてください。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象となるファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクトファイルへのリンクを確認します。これにより、大きな指示セットを小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示はよい出発点ですが、追加すべき内容があります。新たに生成する TypeScript に TSDoc コメントを含めるため、`.github/copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルター機能のコンテキストを保ったまま Copilot CLI を終了して、会話を再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトを使い、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストをどのように取得するかを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスごとの指示が、どのようにエージェントに適用されるかを確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、テストと lint を一貫して実行します。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [Copilot の custom instructions サポート][custom-instructions-support]
- [効果的な custom instructions を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
