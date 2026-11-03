---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルターの動作を確認しました。ここでは、明確に定義した QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールとどのように連携するかを確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能な報告を作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な設定です。プロファイルにはエージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらの機能を要件の評価や結果報告にどう使うかを Copilot に指示します。既存の機能に代わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。実行するチェックと、エージェントが守る境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を正しく取り込んだかレビューします。これにより、検証だけを行いたいときに、エージェントが機能を変更してしまう事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略可能ですが、設定するとエージェントの表示名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更のコミットやプルリクエストの作成を行わないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントへ修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後、同じフィルター機能の会話を再開し、QA を選択して、Issue と計画の内容をコンテキストとして使います。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されないことがあります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトを使い、QA エージェントに機能をレビューしてもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 作業が完了したら、結果の報告を読みます。

## まとめと次のステップ

再利用可能な専門的な役割を追加し、その成果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールとどのように連携するかを確認しました。
- 再利用できる品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に対する結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証結果がそろいました。次は、これらを[機能 PR にまとめ、Agent Merge を使用][next-lesson]します。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
