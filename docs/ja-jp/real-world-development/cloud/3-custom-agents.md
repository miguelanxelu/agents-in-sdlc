---
title: "演習 3 - カスタムエージェント"
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

カスタムエージェントでは、次の内容を確認します。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のフィードバックでは、現在のダークテーマは文字色と背景色のコントラストが不十分で、読みにくいと感じるユーザーがいます。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。内容を読み、Copilot をどのようにガイドするかを確認します。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する主要な責務、アクセシビリティのベストプラクティス、HTML、CSS、JavaScript のコード例、よくある問題と間違いを確認します。

## issue を作成して割り当てる

Mission Control は、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加の指示を出せます。まず、ハイコントラストモードの作成を Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Web サイトにハイコントラストモードを追加する` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   サイトにハイコントラストモードが必要です。ユーザーがハイコントラストを切り替えられるようにし、ブラウザーの local storage に設定を保存してください。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントと Accessibility が選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学び、特定のタスクや分野に合わせた AI アシスタントを確認しました。カスタムエージェントを使うと、チームの知識や標準を再利用可能な形にまとめ、Copilot が作業をより適切に進めるようガイドできます。

この演習では、次の内容を確認しました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot がハイコントラストモードを実装している間に、[Mission Control からエージェントセッションを監視して調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
