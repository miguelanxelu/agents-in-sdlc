---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、技術的負債が課題になっています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエストを作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent について、次の内容を確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作成した開発者がドキュメントを適切に整備しておらず、実質的に何もありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在の阻害要因ですが、優先度の高い別の issue があります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業をしている間に非同期で処理できます。後で結果をレビューし、期待どおりであることを確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境を準備して、タスクに取り組みます。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで開発者に通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業にも、詳細を掘り下げると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、作業を調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の動作をガイドする方法を確認しました。カスタム指示のハンズオンを完了していれば `.github/copilot-instructions.md` を使ったことがあるはずです。まだの場合は、ここで簡単に確認します。cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認し、後で効果を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `コードにドキュメントが不足している` issue を cloud agent に割り当てます。生成されたプルリクエストで、これらの指示に由来する TSDoc コメント、TypeScript の規約、コメントヘッダーを確認してください。後の演習で PR をレビューするときにも触れます。

## Copilot cloud agent 用の開発環境をセットアップする

誰が作業する場合でも、コードを作成するには特定の環境と、状態を整えるセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行する処理をカスタマイズできます。必要な開発ツールや依存関係を使えるようになります。ワークショップを円滑に進めるため、この環境は事前に構成されています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最低限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ中にリポジトリをクローンする場合 (依存関係のインストールなど) は `contents: read` が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ。アプリ全体は Astro と Drizzle/libSQL で構成されています。
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションし、シードデータを追加します。ビルドとテストでデータを使います。
      - name: Set up the database
        run: npm run db:setup
```

他の GitHub ワークフローファイルと同様ですが、次の点に注意してください。

- `copilot-setup-steps` という1つのジョブがあります。このジョブは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古い、または不足しているプロジェクトは少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメント作成にも役立つため、この issue を Copilot cloud agent に割り当てます。バックグラウンドで必要なドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `コードにドキュメントが不足している` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   組織では、役立つ場合に関数やメソッドへ TSDoc のドキュメントコメントを付けることを求めています。しかし、最近の更新はこの標準に従っていません。動作を明確にするドキュメントコメントが含まれるよう、既存のコードを更新する必要があります。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: コードにドキュメントが不足している` のような内容です。一覧に PR がまだ表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかる場合があります。Copilot は、先ほど説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. リストを確認し、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて判断した後、コードを作成します。

この作業には数分かかります。Copilot cloud agent の主な利点は、タスクを非同期で実行し、開発者が別の作業に集中できることです。2つ目のタスクを cloud agent に割り当てる間に、アプリの機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは作業を分けられることです。デザインチーム向けに関連ゲームの表示を追加する作業が短時間でも、別のタスクに使える時間を確保できます。これも Copilot cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `ゲーム詳細ページに関連ゲームを表示する` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   各ゲームの詳細ページに関連ゲームを表示し、ユーザーがさらにゲームを見つけられるようにします。デザインチームは UX を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータアクセスヘルパーを追加します。指定したゲーム自体を除き、同じカテゴリに属する他のゲームを返します。
   - そのヘルパーを使う「関連ゲーム」セクションをゲーム詳細ページに表示します。
   - 関連ゲームがない場合にも対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す目の絵文字 👀 が issue の最初のコメントに表示されます。

![Copilot が issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューします。次にカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当て、非同期で処理させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使います。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
