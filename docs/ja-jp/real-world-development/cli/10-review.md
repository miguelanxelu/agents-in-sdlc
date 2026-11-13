---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用できるカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、事前に登録されたフィルター機能の issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選び、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能全体をレビューし、フィルター機能の PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## 公開した変更

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示をゲームカードに追加します。
2. **フィルター機能と品質改善:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

ワークショップを簡略化するため、フィルター機能の計画から PR 作成まで同じ会話とブランチを使い、変更を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP による Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は要件とテスト範囲のレビューをこれらの結果と組み合わせました。

追加したテストで実際の不足を補うことが重要です。QA の実行で追加テストが不要と判断される場合もあります。マージを承認する前にコードと証拠を確認し、変更後は影響する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方法を検討するとき、**Interactive** は対象を絞った変更を進めながら確認するとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。日常的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主なワークフローを学びました。Copilot CLI には、ほかにも役立つ機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで活用方法が身につきます。製品コード、趣味のコード、作ろうと思いながら何年も手をつけていなかった小さなアプリなどで試してみてください。学んだことをチームと共有し、チームの知見も取り入れましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
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
