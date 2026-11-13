---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」進めるかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI ハーネスとは異なり、この演習では更新前と更新後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys では、開発に関するガイドラインと要件を定めています。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発プラクティスに沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。規約、ライブラリ、コードに含めるコメントなどを指定して、より適切な提案を得られます。リポジトリ全体、または特定のファイル種別に適用する指示を作成できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発している内容、ベストプラクティスなど、Copilot Chat や CLI でのほとんどの依頼に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの指示です。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストなどに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格に基づく指示ファイルにも対応しています。詳細は[指示ファイルに関する説明][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。サンプルプロジェクトには、一般的な方法を示す例が含まれています。

- `copilot-instructions.md` には、構築するもの、プロジェクト構成、全体的なコーディング標準など、プロジェクト共通の指針を記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確な指針と、コードの望ましい例・望ましくない例を示します。

指示ファイルの作成方法にも、AI の使い方にも、決まった唯一の方法はありません。プロジェクトで試しながら、効果的な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイル一式を用意してください。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって操作が異なります。各環境のレッスンで必要な場合に説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読み、Copilot に提供されるプロジェクトのコンテキストを確認します。ファイルは GitHub の Web UI または起動済みの Codespace で開けます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認します。これらの内容は Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とする glob を指定します。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルに適用されます。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分割し、コード生成で参考にする例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイルコメントのヘッダーに関する規則を追加します。

## リポジトリ標準を追加する

ここで編集するのは、コード内にドキュメントを記載するためのプロジェクト全体の規則です。cloud agent は、このハーネスの後半で割り当てる issue に取り組む際に、この指示を読み込みます。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読みますが、変更を分けてコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を伝えるファイルです。コーディング標準を記載して、コードの提案を改善します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関する規則はありません。そのため、先ほど生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、ファイルコメントのヘッダーと TSDoc コメントを追加する次の Markdown 行を追加します。

   ```markdown
   - エクスポートするすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを付けます。
   - import 文やコードの前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的な指針を示すリポジトリレベルの `.github/copilot-instructions.md`、または特定の言語、ファイル種別、タスクに向けた `*.instructions.md` として作成できます。ドキュメントコメントの規則のような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は issue が対象とするブランチの指示ファイルを読みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示を読み込ませるには変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で割り当てる issue に取り組む際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せずに `main` へコミットできます。ここでは、実際のプロジェクトでの作業に近づけるためにブランチを使います。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストに標準が反映されることを確認します。

次は、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot カスタマイズ用の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot に適切なカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
