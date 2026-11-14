---
title: "レッスン 4 - カスタム instructions で Copilot を導く"
description: "リポジトリ instructions を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要があるなら、そのガイダンスを Copilot が利用できるようにします。[Instruction ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も説明できます。フィルター機能を作成したので、Copilot が参照した instructions を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリ instructions とパスに応じた instruction ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう instruction ファイルを更新します。
- instruction ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入する場合は、その引数についても記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

instruction ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## Instruction ファイル

カスタム instructions を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案を得られます。好みの規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル種別に適用する instructions を作成できます。

instruction ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡される単一のファイルです。多くのリクエストに関連するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイドラインを指定するファイルです。

> [!NOTE]
> その他の instruction 形式やサポート状況は環境によって異なります。形式に依存する前に、[カスタム instructions のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム instructions ファイルを確認する

スタータープロジェクトには、すでに複数の instruction ファイルがあります。変更を加える前に内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。このファイルの instructions は、このリポジトリでのすべての Copilot とのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの instruction ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここでは instructions の適用対象を決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の instruction ファイルや既存プロジェクトファイルへのリンクを確認します。大きな instruction セットを再利用可能な小さなファイルに分け、Copilot に参照例を示せます。

## チームのガイダンスに合わせて instruction ファイルを更新する

既存のファイルには不足があります。新しく生成する TypeScript にドキュメントを加えるため、コアの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。およそ35行目にあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリ instructions を読み込みます。更新したガイダンスを適用するため、フィルター機能のコンテキストを保ったまま会話を閉じて再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチにいることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリ instructions とパスに応じた instruction ファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準に従うよう instruction ファイルを更新しました。
- instruction ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` skill を[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム instructions を追加する][instruction-files]
- [カスタム instructions のサポート状況][custom-instructions-support]
- [効果的なカスタム instructions の作成に関するベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
