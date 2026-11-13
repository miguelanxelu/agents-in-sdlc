---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動くことを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部に接続できると、さらに多くの作業ができます。Copilot が外部のツールやサービスと通信するための標準的な方法が Model Context Protocol (MCP) です。

![エージェントモードの内部と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。MCP によって、エージェントはリアルタイムで外部ツールやサービスとやりとりできます。リソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバー経由でアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールと、AI エージェントとの通信を管理します。各 MCP サーバーは、エージェントが利用できる別々のツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスします。リポジトリの作成と更新、issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の検索や貢献を促進する [MCP レジストリ](https://github.com/mcp)を公開しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。使用前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作でき、機能のテストに役立ちます。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、Codespace にはブラウザーを表示するデスクトップ環境がないため、画面を表示せずに実行するよう Playwright に指示します。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使っているため、必要なブラウザーはすでにインストールされていることがほとんどです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを必要とするときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられたら、構成を確認し、サーバーを起動するため信頼することを選択します。

手動で起動する手順は省略できます。その場合、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作しなくても、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、確認した内容を要約できます。会話を離れずに機能を確認する手軽な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を基にエージェントが判断します。支援技術と同じように情報を扱うため、簡単な機能確認がアクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを実行した状態で、Copilot にフィルター機能を試してもらいます。

```text
Playwright MCP サーバーを使って、実行中のアプリ (http://localhost:4321) をブラウザーで開き、新しいゲームのフィルター機能を確認してください。

1. ゲームページに移動し、表示されているゲームの数を確認します。
2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが一覧に表示されることを確認します。
3. フィルターを解除して発行元フィルターを適用し、その発行元のゲームだけが表示されることを確認します。
4. カテゴリと発行元のフィルターを組み合わせ、両方の条件が結果に反映されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らして報告を確認します。問題があれば、追加で質問するか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。不足が報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンドラインインターフェイス (CLI) などで作成できます。ここでは、GitHub Copilot に作成を依頼します。関連する issue を探し、issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリ内でカテゴリと発行元による絞り込みに関する issue を探してください。現在のブランチのプルリクエストを新しく作成し、該当する issue に関連付けてください。
   ```

4. 情報の収集や操作の実行に必要な場合は **Continue** を選択して許可します。GitHub ツールを初めて使用するときは、GitHub への認証を求められる場合があります。画面の指示に従って許可します。
5. Copilot が issue を検索して該当の issue を見つけ、PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 機能を公開する前に、ブラウザーでの手動テストに使用しました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーで機能を**テスト**しましたが、MCP は Copilot を拡張する方法の1つにすぎません。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
