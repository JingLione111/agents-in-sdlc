---
title: "演習 3 - カスタムエージェント"
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

カスタムエージェントでは、次の内容を確認します。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいと感じるユーザーがいることが分かりました。デザインチームは、この問題に対応するため、ユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なので、カスタムエージェントを使って機能をすばやく生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を確認して、Copilot への指示を把握します。

Codespace に戻り、アクセシビリティ用のブランチを `main` から作成して、カスタムエージェントのファイルを確認します。

1. ターミナルを開き、次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` のフロントマターは、カスタムエージェントに必須です。

4. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主要な責任、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題と間違いが説明されています。

## issue を作成して割り当てる

Mission control は、環境のすべてのエージェントを扱う中心的な場所です。Copilot cloud agent にタスクを割り当て、監視し、作業を方向づけたり追加情報を与えたりできます。cloud agent にハイコントラストモードの作成を依頼します。

1. GitHub.com でリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントの割り当て画面で Accessibility を選択したスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、[カスタムエージェント][custom-agents]が GitHub Copilot の特定のタスクや分野に合わせた AI アシスタントであることを確認しました。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot の作業に反映できます。

次の内容を確認しました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot がハイコントラストモードを実装している間に、次は mission control から[エージェントセッションを監視して調整][next-lesson]します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
