---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードを検証し、指示ファイルでコーディング標準に従わせました。テストや lint など、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルは、こうした作業を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次の作業を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使います。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用できるタスクの指示、実行可能なスクリプト、サポート用のリソースがまとめられています。基本的には、スキル名をフォルダー名にし、その中に `SKILL.md` という Markdown ファイルを置きます。Markdown には、スキルの内容を定義する `name` と `description` のフロントマター、機能の概要、呼び出すタイミングの指針を記載します。スキル用のスクリプトや他のリソースを含むサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトの既存コマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有・再利用するリポジトリ資産としてプロジェクトの `.github/skills` に置くことも、ユーザースキルフォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の進め方がどのように説明されているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接、または自然言語を使って呼び出せます。Copilot に3種類のチェックを実行してもらいます。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   quality-checks スキルを使って、ユニットテスト、lint、型チェックを実行してください。
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した項目、成功したかどうか、ツールが実際に報告した内容がわかるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## 結果の出力形式

   完了したら、実行した各コマンドと、成功、失敗、ブロックのいずれかを報告します。テスト数、所要時間、エラー、警告、その他の指標は、ツールが報告した場合にのみ含めます。失敗や阻害要因がある場合は次の対応を明示し、スキップまたは未完了のチェックを成功と記載しないでください。
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

変更を加えたので、スキルを再読み込みして実行します。Copilot CLI では、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   quality-checks スキルを使って、ユニットテスト、lint、型チェックを実行してください。
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次の作業を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルター機能と一緒に機能の PR に含めます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルの使用例

コミュニティによる次の例は参考情報であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
