---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完全なアプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コード作成、動作テスト、プルリクエストのレビュー、エージェントを使った作業管理を段階的に進めます。

## シナリオ

開発者として、ボードゲームのクラウドファンディングを提供する架空の会社 Tailspin Toys に加わります。チームのバックログは GitHub issue として登録済みです。フィルターやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約の改善にも取り組みます。

## 開発環境を選ぶ

GitHub Copilot は、開発者が作業する環境で利用できます。希望する開発方法に合った環境を選択してください。各ワークショップはそれぞれ独自のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instructions、skills、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot アプリ][app]

デスクトップアプリで複数のエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
