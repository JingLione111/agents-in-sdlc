---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数ファイルの変更やテスト実行が必要になることがあります。開発者は通常、関連ファイルを探し、変更し、テストを実行し、問題を修正し、見落としたファイルを見つけて再度更新します。作業は多岐にわたります。

Copilot Agent Mode を使うと、IDE の中でより自律的に作業できます。開発者と同じように既存のプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- GitHub Copilot Agent Mode がユーザーインターフェイス (UI) とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が作成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと販売元で絞り込めるようにします。前の演習では販売元ヘルパーを追加しました。ここでは、残りのデータレイヤー、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter から Drizzle ORM を通じてローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。

1. 前の演習の続きで Codespace に戻ります。`main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. `main` を更新し、フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは、Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

サイトが起動したら、次の主な機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボードゲーム一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、販売元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

サイトは動作していますが、新しい機能を追加したいと考えています。テンプレートからプロジェクトを設定したときに issue のバックログも作成されています。GitHub Copilot に内容を表示してもらいます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** はファイル変更を計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、比較的一貫した体験になることを期待して手順に含めています。ただし、大規模言語モデル (LLM) は確率的に動作するため、提案は記載内容と異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示するコマンドを実行するため、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認します。

## Instruction ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを Copilot に伝える instruction ファイルを確認します。Web サイトに機能を追加する方法を示す [ユーザーインターフェイス (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。これには次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル種別向け instruction へのリンク。

> [!TIP]
> instruction ファイルから、他の instruction ファイルやプロジェクト内のファイルを参照できます。パスは instruction ファイルからの相対パスです。これにより、instructions を再利用しやすく小さな単位に分け、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) の残りのフィルター処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

コードベースにマージする前に、テストが実行されて成功する必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI instruction ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトを使い、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけて変更し、テストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行います。タスクやコードベースの複雑さによっては数分かかります。作業中にエディターで **Keep** と **Undo** が表示されることがあります。完了後はすべての変更に対して操作できるため、作業中に選択する必要はありません。

7. Copilot に求められたら、テストを実行するため **Continue** を選択します。

   ![Copilot Chat のペインでテスト実行の確認を求めるダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot は、エラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加しました。この演習では、次の方法を確認しました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する。
- プロジェクトを調べ、関連ファイルを特定し、連携した変更を行う。
- コードベースにマージする前に、生成された変更とテストをレビューする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 発展演習 – ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの追加を依頼します。次の点を検討してください。

- 既存の instruction ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に追加の変更を依頼し、コンテキストを与えながら反復する必要がある場合があります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
