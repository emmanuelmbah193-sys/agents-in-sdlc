---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュコマンドがあります。これらのコマンドを使うと、高度な機能や内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを調べる

大規模または複雑なタスクに取り組むと、モデルのコンテキストウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って、状況を確認したり自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているか確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きな違いは見られないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関係のない別タスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。一覧を閉じる場合は <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown に出力する `/share file` など、目的に合った出力先を選択します。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を拡張または委任する

ワークショップの主要な内容は完了です。Copilot CLI には、大規模なタスクに役立つ次のコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割して並行実行できます。
- `/worktree` は、別タスク用に独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドを使うと追加の worktree やリモート作業が発生するため、任意の操作としています。新しく範囲を明確にしたタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにもスラッシュコマンドや機能があります。最後に、[学んだ内容を振り返り][next-lesson]、次の学習方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
