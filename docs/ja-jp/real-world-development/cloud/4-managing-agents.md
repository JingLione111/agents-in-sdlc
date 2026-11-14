---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、Copilot cloud agent に UX の改善と機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。作業内容を調べ、進行中のセッションに追加の指示を送ることもできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を送り、追加機能を依頼します。

## シナリオ

エージェントにハイコントラストモードを依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、同じ作業に含めるのが合理的です。エージェントにライトモードも追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。次のようなタスクが表示されます。
   - コードベースのドキュメント更新。
   - ゲーム変更用 API の生成。
   - Web サイトへのハイコントラストモード追加。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次の作業が含まれる場合があります。
   - リポジトリのチェックアウト。
   - Copilot の環境作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するための各種作業。

> [!NOTE]
> 表示される手順は Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業を監視できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認したので、ハイコントラストモードを作業中の Copilot に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot が作業中のエージェントページで、Steer active session while Copilot is working ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り込んだことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。プロンプトを送信した時点で実行していた手順が終わると、新しい依頼を作業に取り込みます。

前と同様に数分かかることがあります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを確認しました。この mission control では、実行中および完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクを調整できます。

次の内容を確認しました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示を送り、追加機能を依頼しました。

Copilot がアクセシビリティ機能を完成させている間に、Copilot が作成したプルリクエストを[反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
