---
title: "演習 3 - Playwright の Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させます。その後、ブランチを公開してプルリクエスト (PR) を作成します。

この演習では、次のことを行います。

- MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## MCP とは

agent mode は、エディターの外部にあるツールを利用すると、さらに強力になります。MCP を使うと、Copilot は外部のツールやサービスと通信できます。

![agent mode の内部構成と、コンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの間を仲介する MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や npm パッケージのようなローカルツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、リポジトリの作成や更新、issue やプルリクエストの管理などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供し、Web ページの移動、フォーム入力、ボタン選択などを行います。

さまざまなツールやリソースを提供する MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促すために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot に操作可能なブラウザーを提供し、機能のテストに使えます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、Codespace のようにブラウザーを表示するデスクトップがない環境で、画面を表示せずに実行するために必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使っているため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザー不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認し、サーバーを起動できるよう信頼します。

手動での起動は任意です。実行しない場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、内容を報告します。会話を離れずに機能を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目などのラベル付き要素を認識して判断します。これは支援技術と同様の構造であり、簡単な機能確認をアクセシビリティの基本確認にも活用できます。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。報告内容を issue の受け入れ条件と照らし合わせます。問題がある場合は、追加の質問をするか、PR 作成前にコード修正を依頼します。

> [!NOTE]
> テストするには、アプリを `http://localhost:4321` で起動しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot の変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

GitHub.com や GitHub コマンドラインインターフェイス (CLI) など、PR を作成する方法はいくつかあります。GitHub Copilot を使用中なので、PR の作成を依頼します。関連する issue を見つけて関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェントドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作の実行を許可します。GitHub ツールを初めて使うときは認証を求められる場合があります。案内に従って許可します。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が表示したリンクを選択して PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前にブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能の PR を作成しました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の1つにすぎません。次は、アクセシビリティレビューなど、目的を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
