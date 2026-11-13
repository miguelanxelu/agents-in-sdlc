---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段使っているコードエディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で、Copilot Chat のエージェントモードを使い、MCP 経由で外部ツールに接続し、カスタムエージェントを活用します。IDE を離れずに作業でき、Copilot はファイル、ターミナル、問題をすべて把握できます。

まずカスタム指示を追加し、Copilot がその指示に従うことを確認します。次に、エージェントモードを使って UI、データ層、テストにまたがるフィルター機能を構築します。続いて Playwright MCP サーバーに接続し、プルリクエストを作成する前に Copilot にブラウザーを操作させて機能をテストします。最後に、アクセシビリティ作業向けのカスタムエージェントを確認して使い、エディターを離れずに Copilot の変更を監視、調整、反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して確認します |
| [2. エージェントモード][ex2] | コード生成 | エージェントモードでフィルター機能を構築します |
| [3. Playwright と MCP][ex3] | 外部ツール | Playwright MCP サーバーを使って、ブラウザーで機能をテストします |
| [4. カスタムエージェント][ex4] | 専門エージェント | カスタムエージェントを確認して使用します |
| [5. エージェントの管理][ex5] | 監視 | エージェントセッションを監視して調整します |
| [6. 反復][ex6] | レビュー | Copilot の作業をローカルでレビューし、次のステップを選びます |
| [オプション: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit を使う3つのモジュールで、モデルを準備し、エージェントをデプロイしてサイトに接続します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、各ハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
