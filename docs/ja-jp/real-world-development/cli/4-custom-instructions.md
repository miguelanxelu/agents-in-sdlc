---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクに決まった手順がある場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys では、開発に関するガイドラインと要件を定めています。

- コメントにはコードを繰り返すのではなく、目的や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数では、`db` 引数を注入できる場合はそれも含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたときにコメントも最新に保ちます。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発プラクティスに沿ってタスクを進められるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。規約、ライブラリ、コードに含めるコメントなどを指定して、より適切な提案を得られます。指示はリポジトリ全体、または特定のファイル種別に適用できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。ほとんどの依頼に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関する指針を示します。

> [!NOTE]
> 他の指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターターのプロジェクトには、作業を始めるための指示ファイル一式が含まれています。変更を加える前に、すでにある指示を確認して効果を比較できるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングの指針を確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存ファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分割し、参考例を Copilot に示せます。

## チームの指針に合わせて指示ファイルを更新する

既存の指示は出発点として役立ちますが、TypeScript に新しく生成されるコードへ TSDoc コメントを追加する規則がありません。コアの `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きに、次の行を追加します。

   ```markdown
   - すべての新しい TypeScript コードに、ドキュメント用の TSDoc コメントを付けます。
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示で会話を再開し、機能のコンテキストを保ったまま Copilot に新しい指針を適用させます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトし、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   会話の選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、新しい指針を適用するよう Copilot に依頼します。

   ```plaintext
   指示とコードの指針を更新しました。生成したコードをその指針に合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、lint とテストを一貫して実行できるよう、再利用可能な [quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
