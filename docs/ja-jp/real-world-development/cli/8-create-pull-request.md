---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、instructions、skill の更新、QA プロファイル、テストをまとめてレビューし、プルリクエストを作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、instructions の更新、skill の更新、品質保証 (QA) プロファイル、テストは、すべて1つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にマージのライフサイクルを管理させます。

> [!NOTE]
> 通常は機能、instructions の更新、skill の更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質ワークフローを同じ会話とブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証結果を調べます。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR のマージを確認します。

## シナリオ

フィルター機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの承認は開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、PR を妨げている問題に Copilot が対応します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは開発者が **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をまとめてレビューし、PR を作成して Agent Merge に残りの手順を任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されていればレビュー済みの PR を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR のマージを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
