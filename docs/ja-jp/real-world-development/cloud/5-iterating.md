---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業内容をレビューする

このワークショップでは、ユーザーエクスペリエンスを改善し、機能を追加する複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の作成、ハイコントラストとライトモードを切り替えるアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善します。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューを始める前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ監視なしで作業するため、安全を保つためのセキュリティ制約があります。

- Copilot がリポジトリに持つ権限は読み取り専用で、書き込みは作業用のブランチに限られます。
- cloud agent は GitHub Actions 内で動作し、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成した、コードへのドキュメント追加のプルリクエスト (PR) を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーなどの外部リソースへのアクセスは既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com でリポジトリを開きます。
2. **Pull Requests** を選択し、プルリクエストの一覧を開きます。
3. **コードにドキュメントが不足している** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は完了を待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントなどのドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。演習 2 の最初に確認したカスタム指示ファイルに基づく、TSDoc コメント、TypeScript の規約、コメントヘッダーに注目します。
6. 変更を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェック一覧に追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやりとりする方法は、変更を一方向に渡すだけではありません。チームメンバーと同様に、PR やコードへのインラインコメントで Copilot に言及できます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確率的に変化するため、プロンプトを決めつけることはできません。次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、他のユーザーと同じように **@copilot** を指定します。上記の例、またはコードベースのドキュメントに関する別の提案を使います。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始することを確認します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで Copilot に変更を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **ゲーム詳細ページに関連ゲームを表示する** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. ワークフローが PR のチェック一覧に追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。以前と同じように、Copilot cloud agent に追加変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます。この issue を割り当てたのは、カスタム指示を更新する前だったことに注意してください。前の PR と同様に、**Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で Mission Control から依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Web サイトにハイコントラストモードを追加する** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は作業の完了を待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持する local storage の使用。
   - ハイコントラストとライトモード向けの CSS またはスタイルの変更。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript / TypeScript コード。

6. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェック一覧に追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。Codespace に移動し、ターミナルで Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントによって、Copilot がアクセシビリティのベストプラクティスに沿って機能を実装したことを確認します。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — さらに作業を委任する

cloud agent は、実際のバックログ項目を割り当てて別の作業に集中するときに役立ちます。習慣にするため、リポジトリに issue をいくつか登録して Copilot に割り当てます。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の作業を行いました。

- このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力への影響を確認できるようにしました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ向けのカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを Mission Control として使い、進行中のアクセシビリティセッションを監視して調整しました。
- Copilot のプルリクエストをレビューして反復し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学ぶ場合は、他のハーネスで今回の内容を広げられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つ公開します。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト範囲の issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールの構成][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
