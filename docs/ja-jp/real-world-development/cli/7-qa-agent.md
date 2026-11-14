---
title: "レッスン 7 - 品質保証 (QA) エージェントを作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる品質保証カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。今度は明確な品質保証 (QA) プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが instructions、skills、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能を issue の要件と照らして確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントで QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する、Copilot の特化した役割です。目的、instructions、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリ instructions はチームの標準を記述し、`quality-checks` skill は繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルはこれらを使い、要件を評価して結果を報告するよう Copilot に指示します。これらの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには QA の確認事項と守るべき境界の両方を記述します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、意図した QA の流れと制約を Copilot が取り込んだことを確認します。これにより、検証だけを依頼したのにプロファイルが不完全だったり、範囲が広すぎて機能を変更したりするのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めると表示名が明確になります。
3. プロファイルの instructions を読み、QA が要件を起点とし、リポジトリ instructions に従い、`quality-checks` skill を実行して Playwright MCP を利用することを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたり PR を作成したりしないことを確認します。
5. 不足している役割や制約があれば、続ける前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択して issue と計画の内容を引き継ぎます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、選択可能な一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進める様子を確認します。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その結果を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが instructions、skills、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能を issue の要件と照らして確認しました。

実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらを機能 PR にまとめ、Agent Merge を使います。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
