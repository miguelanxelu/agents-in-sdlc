---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザーエクスペリエンスの改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進捗を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]もその1つです。この Mission Control では、オープンなプルリクエスト (PR) があるエージェントタスクをすべて確認できます。実行された操作を調べ、進行中のセッションに追加の指示を出して、作業を導くこともできます。

このレッスンでは、次の作業を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するとよいと考えました。サイトのスタイルを更新し、切り替え機能を追加しているため、同時に実装するのが合理的です。エージェントの作業を調整し、ハイコントラストとライトモードの両方を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。Copilot に割り当てた次のタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを変更する API の生成。
   - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような作業が表示されます。
   - リポジトリからのコードのチェックアウト。
   - Copilot が作業する環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクの完了に向けた各種操作。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業をさらに監視できます。

## cloud agent の作業を調整する

実行中のタスクを確認したので、ハイコントラストモードを作成している Copilot にライトモードも依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。表示されるタイトルは、Copilot が選んだ名前や作業状況によって異なります。

   ![Mission Control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   ハイコントラストモードに加えて、ライトモードも追加してください。ユーザーが希望する表示モードを選べるように、ライトモードの切り替えも用意してください。
   ```

   ![Copilot が作業中のエージェントページで、Steer active session while Copilot is working ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映することを確認します。

## Copilot の作業を続ける

更新されたタスクに基づいて、Copilot が作業を続けます。メッセージを送信した時点で作業中だった手順を完了した後、新しい依頼を進行中の作業に取り込みます。

この作業にも数分かかります。その間に、ここまでに学んだことを振り返ってください。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページを確認しました。この Mission Control では、進行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクの範囲や指示を変更できます。

このレッスンでは、次の作業を行いました。

- Mission Control とエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
