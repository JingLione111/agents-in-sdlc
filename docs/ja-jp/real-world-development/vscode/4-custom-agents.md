---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、具体的な指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントとしてまとめられます。たとえば、[ウェブコンテンツアクセシビリティガイドライン (WCAG)][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディング慣行に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、skill はツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一環として skills や他のエージェントを調整します。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフローやテスト実行を管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI の他の機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいと感じるユーザーがいることが分かりました。デザインチームは、この問題に対応するため、ユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なので、カスタムエージェントに機能を生成させ、迅速に実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を確認して、Copilot への指示を把握します。

Codespace に戻り、ターミナルを開いて、フィルター PR とは分けてアクセシビリティ作業用の新しいブランチを `main` から作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` のフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主要な責任、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題と間違いが説明されています。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントドロップダウンには、`.github/agents` で定義された各カスタムエージェントが表示されます。エージェントを選ぶと、そのエージェントの instructions とツールを使う会話を始められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを開きます。現在のエージェントが表示されています。既定では **default** です。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。変更はファイルパスと差分とともにチャット内に表示されます。
6. エージェントが変更したファイルに応じて、**Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。

次の演習では、進行中の作業をレビューし、エージェントに追加の指示を出します。

## まとめと次のステップ

このレッスンでは、[カスタムエージェント][custom-agents]が GitHub Copilot の特定のタスクや分野に合わせた AI アシスタントであることを確認しました。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot の作業に反映できます。

次の内容を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat agent mode でカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して調整し][next-lesson]、変更をレビューしながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
