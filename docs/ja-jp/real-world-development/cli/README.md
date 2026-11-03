---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認します。
2. 星評価の変更を加えてブラウザーでレビューし、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から始め、Plan モードで方針を定め、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. QA カスタムエージェントを作成して使い、要件、テストカバレッジ、検証の証拠をレビューします。
8. フィルター機能全体をレビューし、Agent Merge を使ってフィルター PR をマージします。
9. コンテキスト、モデル、共有、任意の cloud delegation に役立つスラッシュコマンドを確認します。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能の 2 つの PR を作成します。フィルター機能と品質ワークフローでは、1 つの会話とブランチを共有するため、各ツールの使い方を学びながら、それまでの作業を活用できます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認します |
| [2. 星評価の追加で小さな成果を得る][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を追加し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue の機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示による Copilot のガイド][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 繰り返し実行できるチェック | 既存スキルを確認し、報告形式を変更して実行します |
| [6. Playwright MCP による機能の検証][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルターの動作を調べます |
| [7. QA エージェントの作成と使用][ex7] | 要件とカバレッジ | 専門家のプロファイルを作成して選び、最終検証の証拠を集めます |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使用します |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認します |
| [省略可能: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が Copilot CLI を有効にしていることを確認してください。

## 始める

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
