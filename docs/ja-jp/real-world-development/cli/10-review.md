---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の開発作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されているフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム instructions でエージェントを導き、既存の `quality-checks` skill をカスタマイズし、それを使って単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、skill の結果、ブラウザーでの根拠を評価しました。
- フィルター機能全体をレビューし、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップには、更新済みの `main` からそれぞれ別のブランチを使う2つの PR の節目があります。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示をゲームカードに追加します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、instructions を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を1つの PR にまとめています。

## 検証方法の違い

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` skill は単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は PR 作成前に、それらの結果と要件、テストカバレッジのレビューをまとめました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠をレビューし、変更後は関連する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、instructions を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。instructions は標準を設定し、skills は繰り返し可能なタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけではなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は範囲を絞った変更に関与し続けるとき、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力が大きく変わります。

## さらに確認する

基本的なワークフローを終えました。Copilot CLI では、次の機能も確認できます。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。本番コード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してください。チームで知見を共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境を試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリ ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
