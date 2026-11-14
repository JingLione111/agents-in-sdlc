---
title: "演習 1 - カスタム instructions (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝える必要がある場合は、そのコンテキストを参照できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。中でも[instruction ファイル][instruction-files]は、コード生成の指針を伝える方法です。

この演習では、次のことを行います。

- リポジトリのカスタム instructions とパスに応じた instruction ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の instructions を使って、フィルター機能の最初のデータ部分 (販売元ヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。
- 同じプロンプトを再度実行し、生成コードが新しい標準に従う様子を確認します。
- instructions の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新済みガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れることがあります。Copilot の動作は確率的です。この演習では、出力を一字一句一致させるのではなく、instructions の更新後に傾向がどう変化するかを確認します。

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

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡されます。技術スタック、開発対象の概要、ベストプラクティスなど、チャットや CLI の多くのリクエストに関係する情報を記載します。
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

このリポジトリには、コアの `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの instructions を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの相対 glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の instruction ファイル (例: `unit-tests.instructions.md`) や既存ファイルへのリンクを確認します。instruction セットを再利用可能な小さなファイルに分け、Copilot に参照例を示せます。ここに記載されたパスはリポジトリのルートではなく、instruction ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc doc コメントとファイルヘッダーコメントのルールを追加します。

## 変更用ブランチを作成する

変更を分離するため、新しいブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instructions を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム instructions の影響を確認するため、まずは現在の instruction ファイルのままプロンプトを送信します。後で instruction ファイルを更新し、同じプロンプトを再送信して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーとテストなどのコードを変更します。
6. 提案されたヘルパーが、型指定された関数として `db` クライアントを最初の引数に受け取り、販売元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の慣行によるものです。
7. 提案コードに TSDoc doc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加する可能性があります。その場合も問題ありません。instructions 更新後の一貫性の向上を確認することが目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を提供するファイルです。コード提案を改善するため、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクションを探します。およそ27行目にあります。コーディング標準の記載はありますが、コード内ドキュメントのルールはありません。
3. 既存の標準の直後に、ファイルヘッダーコメントと TSDoc doc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction ファイルは、`.github/copilot-instructions.md` のようにリポジトリ全体に適用することも、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` として作成することもできます。doc コメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案ファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に適用し、次の演習で使用するコードに反映できました。

## instructions とブランチをコミットしてプッシュする

instruction ファイルも他のリポジトリアセットと同じように、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. ターミナルで次を実行し、instructions とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新とヘルパーの基礎となる変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。GitHub ツールを使える Copilot に依頼して、issue を探し、PR を作成してマージします。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこれが初めてです。プロジェクトテンプレートには GitHub ツールがあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code でサインインが求められる場合があります。案内に従って許可してください。後の演習では、この接続に使われる Model Context Protocol について説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. instructions ファイルの更新と販売元ヘルパーの基礎を説明し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成する様子を確認します。
5. Copilot に GitHub 操作を許可するよう求められた場合は、コマンドを確認してから必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドを許可するよう求められた場合は、コマンドを確認してから必要に応じて **Allow** を選択します。

GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が instruction ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の instructions を使ったコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案コードが新しい標準に従うことを確認しました。
- instructions とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにしました。

次は、[agent mode][next-lesson]で instructions を活用し、Copilot にコードベース全体で新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム instructions 作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム instructions を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム instructions][personal-instructions]
- [Awesome Copilot — instruction ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
