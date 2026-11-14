---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム instructions][previous-lesson] |
|:--|

多くの組織では、技術的負債に直面しています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。これにより、手間のかかるタスクを任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent で次の内容を確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に実行する方法。
- issue の範囲を明確に定義する重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを作成した委託先は、ドキュメントを十分に作成しませんでした。まず、アプリケーションのすべてのエクスポート関数に TSDoc doc コメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示し、閲覧を続けられるようにします。現時点では完成された実装ではなく、ユーザーエクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業はブロッカーですが、より優先度の高い issue もあります。

こうした作業は優先順位が下がりやすく、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で処理する間、開発者は別の作業に取り組み、後で結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取り組みます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで開発者に通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作にも、詳しく調べると複雑な面があります。[Copilot cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、適応させます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム instructions

前の演習ではカスタム instructions と、それが Copilot を導く仕組みを確認しました。カスタム instructions の演習をすでに行っている場合は、`.github/copilot-instructions.md` の効果を見たことがあるでしょう。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された instruction ファイルを読み取り専用で確認してください。

リポジトリの GitHub Web インターフェイス、または起動中の Codespace で次のファイルを確認します。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript 規約と TSDoc doc コメントの要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターを確認します。この instructions は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成された PR に TSDoc doc コメント、TypeScript 規約、コメントヘッダーが反映されているか確認できます。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、特定の環境やセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェアエンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を構成します。`.github/workflows/copilot-setup-steps.yml` に設定した[特別なセットアップワークフロー][setup-workflow]を作成し、作業開始前に実行することで、必要な開発ツールや依存関係を利用できるようにします。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードを実行した SQLite データベースを利用できるよう、あらかじめ設定しています。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

他の GitHub ワークフローと同様のファイルですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトで情報が古かったり不足していたりします。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を支援できるため、この issue は cloud agent に適しています。バックグラウンドで必要なドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで GitHub.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは **[WIP]: Code lacks documentation** のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を構築し、前述のワークフローを実行して、プロジェクトを分析し、問題への最適な取り組み方を検討しています。

13. リストを確認し、実行予定のタスクを読みます。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて、コードを作成していきます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、開発者が他の作業に集中できるようにすることです。この利点を活かし、別のタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けられることです。デザインチームが求める関連ゲームの表示は時間のかかる作業ではないかもしれませんが、その時間を他の作業に活用できます。これも Copilot cloud agent に割り当てましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、完了まで常に監視する必要はありません。作業が終わってからレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について確認しました。cloud agent に issue を割り当て、非同期で実行できます。技術的負債の解消、新機能の作成、別フレームワークへのコード移行などを依頼できます。

この演習では、次の内容を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に実行する方法。
- issue の範囲を明確に定義する重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも使えます][cloud-agent-mcp]。前の演習で扱ったカスタム instructions も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
