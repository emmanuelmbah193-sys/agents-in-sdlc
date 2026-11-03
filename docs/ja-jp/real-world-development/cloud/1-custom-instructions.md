---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に背景情報を知っておいてほしい場合は、そのコンテキストを利用できるようにします。[Instruction file][instruction-files] を使うと、何をしてほしいかだけでなく、どのように実行してほしいかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスごとの指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように届くかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のコースと異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub Issue 上で非同期に作業するため、影響をリアルタイムで並べて示すのが難しいためです。このコースの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- コードのドキュメントは、TSDoc の doc comment として記述します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

### Custom instructions

Custom instruction を使うと、コーディングスタイルや要件を理解するためのコンテキストと要望を Copilot に伝えられます。使用するコーディング規約やライブラリ、コードに含めたいコメントの種類などを指定でき、より関連性の高い提案やコードスニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスクレベルの指示も作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。使用している技術スタック、開発中のものの概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクトレベルのコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの instruction file と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support] もサポートしています。たとえば、AGENTS.md や CLAUDE.md ファイルを利用できます。

### Instruction file を管理するためのベストプラクティス

Instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。ただし、サンプルプロジェクトに含まれる例は、一般的な方法を示しています。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイル種別 (ユニットテスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに関する詳しい指示を記載します。
- 自然言語で明確に指示します。コードの望ましい例と避けるべき例を示します。

Instruction file の作成にも AI の使い方にも、唯一の正解はありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトには、充実した instruction file のセットを用意しましょう。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けのファイルを確認できます。
>
> Copilot に instruction file を作成してもらうこともできます。作業環境によって方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。現在の作業環境に関するレッスンで、必要なときに説明します。
>
> テンプレートや出発点を探す場合は、instruction file、カスタムエージェントなどのリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスにも注目します。これらの指示は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、リポジトリルートからの相対パスに対して、指示の適用先を決める glob を指定します。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` など、ほかの指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用しやすいファイルに分け、コード生成時の参考例を Copilot に示せます。ここに記載されたパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーに関するルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは、プロジェクト全体に適用するルールを追加する 1 か所だけです。ドキュメントを TSDoc の doc comment とファイルレベルのコメントヘッダーとしてコードに含めるようにします。後の演習で Issue を割り当てるとき、cloud agent はこの指示を読み取ります。

編集の前に、作業用ブランチを作成します。cloud agent は Issue が対象とするブランチから指示を読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。リポジトリのコーディング標準を記述し、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、先ほど生成した helper には doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。ファイルコメントヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file はリポジトリ全体のガイダンス用 (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク用の `*.instructions.md` としても作成できます。ここで追加した doc comment のルールなど、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、push、マージする

Cloud agent は、Issue が対象とするブランチの instruction file を読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を読み取れるよう、変更を `main` にマージする必要があります。

1. 変更を stage してコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で Issue に割り当てたとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して `main` に直接コミットしてもかまいません。実際のプロジェクトでの進め方に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がどのようにコンテキストを取得するかを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このコースの後半では、cloud agent が作成したプルリクエストで、この標準が使われていることを確認します。

次は、[Copilot cloud agent に最初の Issue を割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction file][instruction-files]
- [効果的な custom instructions を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot により良い指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
