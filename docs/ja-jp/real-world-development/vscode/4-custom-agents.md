---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、目的に合わせた指示、ベストプラクティス、コーディングパターン、専門知識を定義します。チームの知識を再利用できるエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` または全体で使う `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。違いを簡潔に表すと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。この演習では、アクセシビリティのカスタムエージェントがサイトをアクセシビリティガイドラインに照らしてレビューし、更新します。その際、プルリクエストのワークフロースキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のフィードバックでは、現在のダークテーマは文字色と背景色のコントラストが不十分で、読みにくいと感じるユーザーがいます。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

この演習では、次の作業を行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。内容を読み、Copilot をどのようにガイドするかを確認します。

Codespace に戻り、ターミナルを開いて、フィルター機能の PR とは分けてアクセシビリティ作業用のブランチを `main` から作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する主要な責務、アクセシビリティのベストプラクティス、HTML、CSS、JavaScript のコード例、よくある問題と間違いを確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択メニューには、`.github/agents` で定義したすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、その指示とツールに合わせて Chat セッションの範囲が設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

1. Chat ビューのエージェント選択メニューで、まだ選ばれていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択メニューのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを開きます。ここには現在のエージェントが表示されます。既定値は **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   サイトにハイコントラストモードを追加してください。ユーザーがハイコントラストを切り替えられるようにし、ブラウザーの local storage を使ってページの再読み込み後も設定を保持してください。
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトファイルを編集します。編集内容は、ファイルパスと差分とともに Chat 内に表示されます。
6. エージェントが変更したファイルの一覧は、**Files changed** の表示で確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに変更を加え、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習で、進行中の作業をレビューして方向を調整します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学び、特定のタスクや分野に合わせた AI アシスタントを確認しました。カスタムエージェントを使うと、チームの知識や標準を再利用可能な形にまとめ、Copilot が作業をより適切に進めるようガイドできます。

この演習では、次の内容を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は、同じセッションにライトモードの切り替えを追加しながら、[エージェントの作業を監視して調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
