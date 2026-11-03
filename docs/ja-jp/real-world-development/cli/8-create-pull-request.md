---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理してもらいます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質ワークフロー全体を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでのプロセスを自動化する方法を学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、開発者がマージの承認権限を維持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能な状態にするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗の修正、レビューコメントへの対応、必要な場合のリベースなど、PR を妨げている問題を解決します。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその処理を任せられますが、有効にする前に作業をレビューし、マージしてよい状態であることを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したら、変更をレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を入力して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの PR を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでのプロセスを自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使う[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
