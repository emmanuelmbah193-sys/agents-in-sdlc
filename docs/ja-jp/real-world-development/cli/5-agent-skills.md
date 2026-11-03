---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、instruction file を使って標準に沿うようにしました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には **agent skill** が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、報告形式の改善を希望しています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本となるのは、スキル名のフォルダーと、その中の `SKILL.md` という Markdown ファイルです。Markdown には、スキル名と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、スキルが呼び出されたときに使用するスクリプトやその他のリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用できるようにプロジェクトの `.github/skills` フォルダーに配置することも、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって、Copilot がスキルを呼び出すタイミングを判断できることを確認します。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI では、スキルを直接呼び出すことも、自然言語を使うこともできます。3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示される報告を確認します。

## 報告をカスタマイズする

何が実行され、成功したかどうか、ツールが実際に何を報告したかが分かる報告に改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを使ってみましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後の報告を確認し、最初の報告と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能の PR に含めます。次は、Copilot がサイトを直接操作できるように、[Playwright MCP サーバー][next-lesson]を使います。

## スキルのその他の例

以下のコミュニティ例は参考資料であり、追加タスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
