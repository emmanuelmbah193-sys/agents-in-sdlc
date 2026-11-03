---
title: "レッスン 6 - Playwright MCP による機能の検証"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を増やす必要もあります。そこで MCP サーバーを使います。MCP サーバーを使うと、CLI に組み込まれている機能を超えて、Copilot が追加のツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を調べます。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Web サイトを利用できるよう Copilot に権限を与えることで、変更が期待どおりに動作する確信を高め、作業をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部ツールやサービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部ツールやサービスの間の橋渡しをします。各 MCP サーバーは、AI エージェントが利用できるツールとリソースのセットを提供します。

広く使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーが利用できます。GitHub は、エコシステム内での発見や貢献を促すために [MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用する前に、ソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップの手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバータイプが **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認してもらう

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを使うように指示しなくても、Copilot が自動的に判断します。ただし、使用するツールが分かっている場合は指定すると、結果の一貫性が高まり、トークンを節約できます。

2. Copilot がブラウザーで実行する確認手順を見ます。
3. 報告を読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- Playwright MCP サーバーが未設定の場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を調べました。

次は、スキルとブラウザーツールを組み合わせる [QA カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
