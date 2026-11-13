---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションです。エージェントを使った開発を1つの専用ワークスペースにまとめ、エージェントセッションの並列実行、セッションモードの切り替え、共有 Canvas、GitHub issue とプルリクエストのネイティブ管理を利用できます。**Agent Merge** は、リベース、レビューへの対応、継続的インテグレーション (CI) の修正、マージまでプルリクエストを進めます。

このワークショップでは、Tailspin Toys で一連のワークフローを進めます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリに接続し、ワークスペースと事前に用意されたバックログを確認します。
2. 星評価の変更に取り組み、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の issue から始め、**Plan** モードでアプローチを決め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、テスト範囲、検証の証拠をレビューします。
8. フィルター機能の変更全体をレビューし、2つ目の PR に Agent Merge を使用します。
9. Canvas が利用できるか確認してから、Database Explorer を使い、リポジトリを基盤とするトリアージ Canvas を作成してテストします。必要な機能を利用できない場合は、阻害要因を記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップに集中できるよう、PR は2つ作成します。1つ目は星評価、2つ目はフィルター機能と指示、スキル、QA プロファイル、テストの更新です。各作業は更新済みの `main` から始めます。フィルター機能と品質改善のワークフローでは、同じセッション、worktree、ブランチを使います。これにより、各ツールを学びながら作業を積み重ねられます。最後の Canvas 演習は同じセッション内で行い、PR ワークフローを繰り返さずに、共有画面の作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトのコピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトに接続し、ワークスペースを確認します |
| [2. 星評価の追加: 小さな成果][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を行い、PR 1 をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | Customize で MCP を設定し、フィルターの動作を調べます |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門エージェントのプロファイルを作成して選び、最終検証の証拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | フィルター、指示、スキル、QA プロファイル、テストをレビューし、2つ目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリを基盤とするトリアージ Canvas を作成してテストします |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、関連リソースを振り返ります |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、各ハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。そのため、[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用する場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。この設定は Copilot CLI のポリシーとは別です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
