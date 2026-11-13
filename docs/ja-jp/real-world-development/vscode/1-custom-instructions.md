---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に情報を伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。コード生成の指針を記述できます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を適用した状態で、フィルター機能の最初のデータ部分 (発行元ヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再度実行し、生成コードに新しい標準が反映されることを確認します。
- 次の演習で更新後の指示を使えるよう、指示の変更とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力が文字単位で一致するかではなく、指示を更新した後の動作の変化を確認します。

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

このリポジトリに含まれる指示ファイルを読み、Copilot に提供されるプロジェクトのコンテキストを確認します。ファイルはエディターまたは GitHub の Web UI で開けます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認します。これらの内容は Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とする glob を指定します。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルに適用されます。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分割し、コード生成で参考にする例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイルコメントのヘッダーに関する規則を追加します。

## 変更用ブランチを作成する

変更を加える前に、ベストプラクティスに従ってブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。その後、指示を更新し、同じプロンプトをもう一度送って結果を比較します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディターのタブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューから **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての発行元について名前と ID の一覧を返すデータアクセスヘルパーを追加してください。ファイルに変更を適用してください。
   ```

5. Copilot がプロジェクトを調べてコードを変更します。ヘルパーファイルとテストに変更が及ぶ場合もあります。
6. 提案されたヘルパーが、型付きの関数であることを確認します。最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約に基づくものです。
7. 提案されたコードには、TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、明示されていなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作が一貫することが、この演習の目的です。

## リポジトリ標準を追加する

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

## 同じプロンプトを再度実行して変化を確認する

指示を更新した状態で同じプロンプトを実行し、提案されるコードに変化があるか確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての発行元について名前と ID の一覧を返すデータアクセスヘルパーを追加してください。ファイルに変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys のクラウドファンディングプラットフォーム用の発行元データアクセスヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべての発行元の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに反映できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の変更と、ヘルパーの基盤に関して Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの関連変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。接続済みの GitHub ツールを使って、issue を探し、issue を閉じる PR を作成してマージするよう Copilot に依頼します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは、GitHub ツールがワークスペースに接続済みです。セットアップは不要ですが、Copilot が初めてツールを使う際、VS Code で GitHub へのサインインが求められる場合があります。画面の指示に従って許可してください。後の演習で、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルター機能で使う発行元ヘルパーの基盤の両方を PR に記載するよう伝えます。

   ```
   指示ファイルの更新に関する issue を探してください。現在のブランチから PR を作成し、その issue をクローズすることを明記してください。また、今後のフィルター機能のために発行元ヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が issue を探して PR を作成するのを待ちます。
5. Copilot に GitHub 操作の許可を求められた場合は、実行内容を確認してから **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻して最新のコードを取得するよう依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新状態にしてください。
   ```

7. GitHub 操作やシェルコマンドの実行許可を求められた場合は、実行内容を確認してから **Allow** を選択します。

GitHub Copilot を使って、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を適用したコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、新しい標準が提案コードに反映されることを確認しました。
- 次の演習で使えるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェントモードで指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot カスタマイズ用の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot に適切なカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
