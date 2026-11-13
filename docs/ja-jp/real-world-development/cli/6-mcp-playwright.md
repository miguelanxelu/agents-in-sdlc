---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスと連携し、Copilot に追加の自動化機能を使わせる必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーは Copilot CLI に組み込まれていないツールやサービスを提供します。

このレッスンでは、次の作業を行います。

- Model Context Protocol (MCP) と Copilot CLI での使用方法を確認します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにし、期待どおりの変更をより確実に作成できるようにします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部ツールやサービスと通信し、最新情報にアクセスしたり、開発者に代わって操作を行ったりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバー経由でアクセスします。各 MCP サーバーは、エージェントが利用できる別々のツールとリソースを提供します。

広く使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスします。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の検索や貢献を促進する [MCP レジストリ][mcp-registry]を公開しています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を構成済みの場合は、このセットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類 **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトで、Playwright MCP サーバーを使って機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らしながらフィルター機能を確認してください。変更を加えずに、動作する点としない点を教えてください。確認が終わったら、自分で起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自分で選ぶため、使用するよう明示する必要はありません。ただし、使用するツールがわかっている場合は、案内すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を確認します。
3. 報告を読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。このレッスンでは、次の作業を行いました。

- Model Context Protocol (MCP) と Copilot CLI での使用方法を確認しました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は、スキルとブラウザーツールを専門的な役割にまとめる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
