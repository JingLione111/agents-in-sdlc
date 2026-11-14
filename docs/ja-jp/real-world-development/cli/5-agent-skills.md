---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルター機能を検証するために実行します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業があります。これまではコードを手動で検証し、instruction ファイルを使って規約に従うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には、**agent skill** が適しています。skill は、こうした処理を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果レポートの形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## Instructions、スクリプト、リソース

Agent skill は、再利用可能なタスク用 instructions、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、skill 名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、skill の名前と説明を定義するフロントマター、機能の概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、スクリプトやほかのリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

skill は、チームで共有・再利用できるリポジトリアセットとしてプロジェクトの `.github/skills` に配置するか、ユーザーの skill フォルダー `~/.copilot/skills` に配置できます。

## skill を確認する

Tailspin Toys のチームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` skill を確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって、Copilot が skill を呼び出すタイミングを判断しやすくなります。
3. instructions を読み、テストと lint の手順をどのように Copilot に示しているか確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot CLI では、自然言語を使うか、skill を直接呼び出せます。まず、3つのチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが報告した内容を示す、より分かりやすいレポートを作成するよう skill を更新します。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新した skill を実行する

変更を加えたので、Copilot CLI が skill を再読み込みできることを確認して、更新内容を試します。会話を再起動せずに、編集済みの skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` skill を確認しました。
- 結果レポートの形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、フィルター機能のプルリクエスト (PR) に含めます。次は、[Playwright Model Context Protocol (MCP) サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## skill の例

コミュニティが作成した次の例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
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
