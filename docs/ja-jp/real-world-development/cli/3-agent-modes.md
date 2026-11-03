---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で Issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って適切なものを適切な方法で構築できるように設計されています。このレッスンから、一般的なエージェント主導の開発プロセスに沿って進めます。まず Issue を使って新機能を作り、コードの妥当性と機能の動作を確認し、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能開発を進める間は、同じ会話とブランチを使います。通常、作業するファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため、一部を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードを使って新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の開発を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリーや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせなど、コーディング前に合意が必要な詳細があります。Plan モードでその判断を整理し、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した Issue から始めます。
2. 構築するものの計画を立てます。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な内容はチームや組織によって異なりますが、ほとんどの場合は上記の流れを調整したものになります。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ検証プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントは計画を作成しますが、プロジェクトファイルは編集できません。
- **Autopilot**: エージェントはコードを書き、テストを実行し、タスクが完了するまで自律的に作業を反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能に着手する前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) を会話のコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、方針を決められます。複雑な作業では、少し時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はまずプロジェクトを調査し、次に適切な実装方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に基づいて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が作成され、確認も終わりました。次は Autopilot モードで Copilot に実装してもらいます。

Autopilot を使うと、Copilot は完了したと判断するまで作業を反復できます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同様の名前の選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業を進め、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。計画を先に確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

ほかのコードと同様、生成されたコードはマージ前にレビューする必要があります。コードを確認し、サイトを起動して、見た目と動作を検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容に照らし合わせます。
4. レビューが終わったら、<kbd>Esc</kbd> を押して diff 表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードは問題なさそうです。実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリーフィルター、発行元フィルター、および計画時に決めた組み合わせを試します。
4. リセット時と結果が空の場合の動作が、Issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを Copilot に停止してもらいます。

## まとめと次のステップ

異なる会話モードを使って、機能を構築してレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、カスタム指示を使って、[コードが文書化されたプラクティスに沿って生成されるようにする][next-lesson]方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
