---
title: "演習 1 - カスタム instructions (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝える必要がある場合は、そのコンテキストを参照できるようにします。[Instruction ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム instructions とパスに応じた instruction ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここでは instructions を更新する前と後で同じプロンプトを実行しません。Copilot cloud agent は GitHub issue を非同期で処理するため、影響をリアルタイムで並べて示すのが難しいためです。この環境の後半で、cloud agent が作成したプルリクエストをレビューするときに、instruction ファイルの効果を確認します。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードで、現代的な見た目にします。
- TSDoc doc コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

instruction ファイルを使い、Copilot がこれらの慣行に沿ってタスクを進められるようにします。

### カスタム instructions

カスタム instructions を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案を得られます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル種別に適用する instructions を作成できます。

instruction ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡されます。技術スタック、開発対象の概要、ベストプラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの作成などのガイドラインを記載します。

> [!NOTE]
> IDE では、instruction ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の instruction ファイルも[サポートしています][custom-instructions-support]。

### Instruction ファイルを管理するためのベストプラクティス

サンプルプロジェクトには、代表的な構成例が含まれています。

- `copilot-instructions.md` には、開発対象やプロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けの指示を記載します。
- 自然な言葉で、明確なガイダンスを記載します。望ましいコードと望ましくないコードの例を示します。

instruction ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、十分な instruction ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot を使って instruction ファイルを生成することもできます。画面によって操作は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要な場合は、各環境の演習で操作を説明します。
>
> テンプレートや出発点を探す場合は、instruction ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム instructions ファイルを確認する

このリポジトリには、コアの `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。GitHub の Web UI または Codespace で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの instructions を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの相対 glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の instruction ファイル (例: `unit-tests.instructions.md`) や既存ファイルへのリンクを確認します。instructions を再利用可能な小さなファイルに分け、Copilot に参照例を示せます。ここに記載されたパスはリポジトリのルートではなく、instruction ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc doc コメントとファイルヘッダーコメントのルールを追加します。

## 新しいリポジトリ標準を追加する

この演習で編集するのは、コード内に TSDoc doc コメントとファイルレベルのコメントヘッダーを記述するプロジェクト全体のルールです。次の演習で issue を cloud agent に割り当てると、このガイダンスが使われます。

編集の前にブランチを作成します。cloud agent は issue の対象ブランチから instructions を読み取りますが、ここでは変更をきれいにコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

次に、`.github/copilot-instructions.md` にプロジェクト全体の標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。およそ27行目にあります。コード内ドキュメントのルールがないため、生成されたヘルパーに doc コメントがありませんでした。
3. 既存の標準の直後に、ファイルヘッダーコメントと TSDoc doc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction ファイルは、`.github/copilot-instructions.md` のようにリポジトリ全体に適用することも、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` として作成することもできます。doc コメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## instruction の更新をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチから instruction ファイルを読み取ります。次の演習で issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent に instructions を使わせるには、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. GitHub.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue に割り当てた cloud agent は、この instructions を読み取ります。

> [!TIP]
> ワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` に直接コミットしてもかまいません。ここでは実際のプロジェクトでの手順に近づけるため、ブランチを作成しています。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。この環境の残りの演習で、cloud agent が生成するプルリクエストに標準が反映されることを確認します。

次は、[Copilot cloud agent に初めての issue を割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム instructions 作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム instructions を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
