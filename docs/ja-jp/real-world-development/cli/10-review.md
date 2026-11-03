---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトと用意されたフィルター機能の Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を定め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントの作業を案内し、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- QA カスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能全体をレビューし、フィルター機能の PR に Agent Merge を使用しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには 2 つの PR の節目があり、それぞれ更新済みの `main` から別のブランチを作成します。

1. **星評価:** 既存の `starRating` と未評価の場合の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` の報告をカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、これらの変更を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを確認しました。`quality-checks` スキルでは単体テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR 作成前の確認を支援しました。

追加するテストは、実際に不足している部分を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前に、コードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールによって、作業の結果が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズして、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は構築前に方針を検討するとき、**Interactive** は変更の過程で確認を続けたいとき、**Autopilot** は範囲が明確なタスクを進めるときに使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択できます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えることで、出力は大きく変わります。

## さらに試す

主要なワークフローを終えました。Copilot CLI には、次のような機能もあります。

- `/review` を使うと、コードレビューエージェントに変更を分析してもらえます。
- `/rubber-duck` を使うと、問題について対話し、別の視点を得られます。
- `/fleet` を使うと、独立したサブタスクを並行して調整できます。
- `/worktree` を使うと、別のタスクを分離できます。
- `/delegate` を使うと、Copilot cloud agent にタスクを送信できます。

## 次のステップ

ツールのスキルを向上させる最善の方法は、使い続けることです。本番コード、趣味のコード、ずっと作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、ほかの人からも学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。別の作業環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
