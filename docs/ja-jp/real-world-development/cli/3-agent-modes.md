---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue に基づくフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前のレッスンでは小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って作業できます。このレッスンから、issue を起点に機能を作り、コードの妥当性と機能の動作を確かめ、最終的にプロジェクトへマージする一般的なエージェント駆動の開発プロセスに取り組みます。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能開発を続けながら、リポジトリの instructions を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリや販売元で絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な詳細があります。Plan モードで決定し、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。一般的な開発フローは次のとおりです。

1. 必要な作業を記述した issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、ほとんどは上記の流れを基本にしています。

この標準的な進め方を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューを通るようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、次の操作に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成し、プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画をレビューしてから、Autopilot で実装します。

## issue から始める

フィルター機能に着手する前に、Codespace に戻り、リポジトリとターミナルの準備をします。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を依頼する前に、機能実装やタスクの進め方を決められます。複雑な作業では、事前に計画を練ると効果的です。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 直前の手順で追加した issue の参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルター機能の動作について質問されたら、希望する内容を回答します。選択に正解・不正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装を依頼します。Autopilot は、完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同様の表示のオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まることがあります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前に環境の問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、他のコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を実行し、フィルターの実装とテストを調べます。
3. 結果を issue と計画時の決定事項に照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認したら、アプリを起動して Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、販売元、および組み合わせでフィルターを試します。
4. リセット時と結果が空の場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム instructions][next-lesson] を使って、コードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
