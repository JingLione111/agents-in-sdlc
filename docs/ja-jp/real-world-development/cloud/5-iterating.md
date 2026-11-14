---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot にドキュメントの追加、デザインチーム向けの関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視されずに作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスと、コードを操作するために使うブランチへの書き込みアクセスのみを持ちます。
- Cloud agent は GitHub Actions 内で実行され、独立した一時的な環境を作成して作業します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードにドキュメントを追加するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot の外部リソースへのアクセスは、外部 MCP サーバーへの呼び出しを含めて既定で制限されているため、これは**想定内**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は Copilot が完了するまで待ちます。数分かかることがあります。その間に休憩したり、学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc doc コメントなど、更新されたコードを確認します。実際の変更は異なる場合があります。レビュー時には、演習 2 の冒頭で確認したカスタム instruction ファイルに基づく TSDoc doc コメント、TypeScript 規約、コメントヘッダーに注目します。
6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェックに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックが成功するはずです。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りする方法は、レビューだけではありません。チームメンバーと同じように PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に変わるため、依頼内容を固定することはできません。たとえば次の変更を依頼できます。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションし、生成されたドキュメントへの変更を依頼します。上記の例や、コードベースに追加したい別のドキュメントの提案を使います。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択してプルリクエストに戻ることもできます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、新しいコミットがプルリクエストに表示されます。
5. **Files changed** タブを選択して変更をレビューします。

必要な変更が残っている場合は、Copilot に修正を依頼し、追加されたコミットと差分をレビューします。作業内容を確定するには、次の手順を行います。

1. 変更に問題がないことを確認し、ドラフト PR をレビュー可能な状態に変更します。
2. `main` ブランチに PR をマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue のために Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. ワークフローがプルリクエストのチェックに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックが成功するはずです。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、コメントを追加して更新を依頼し、Copilot cloud agent の作業を反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc doc コメントを追加したりできます。この issue はカスタム instructions を更新する**前**に割り当てたため、後者の依頼が必要になる場合があります。前と同じく、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control から追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の使用。
   - ハイコントラストモードとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェックに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックが成功するはずです。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントが、Copilot の実装をアクセシビリティのベストプラクティスに沿って導きました。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、実際のバックログ項目を任せ、その間に別の作業を進めると効果的です。リポジトリに issue を作成して Copilot に割り当てる場合は、次のタスクを検討してください。

1. ゲーム詳細ページに支援者向け問い合わせフォームを作成します。
2. ゲーム一覧ページにページネーションを実装します。
3. データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent の環境で一連の演習を完了しました。次のことを行いました。

- 後で cloud agent の出力を確認できるよう、リポジトリに用意されたカスタム instruction ファイルを確認しました。
- issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ向けのカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、進行中のアクセシビリティセッションを監視して調整しました。
- Copilot が作成したプルリクエストをレビューして反復し、**@copilot** に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent の環境での作業は完了です。引き続き確認する場合は、他の環境のワークショップも活用してください。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE から Copilot Chat agent mode と MCP 統合を確認します。
- 💻 **[CLI 環境](../../cli/)** — Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つ作成します。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野のカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
