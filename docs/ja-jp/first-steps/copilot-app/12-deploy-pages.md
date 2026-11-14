---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot アプリを使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages にデプロイして、誰でもアクセスできるようにしましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages デプロイの設定を依頼します。
- マージ前にワークフローとプルリクエストをレビューします。
- デプロイの進行を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL からアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストを作成すると、コードがコミットおよびプッシュされることがあります。プロンプトを送る前に、承認設定と送信先リポジトリを確認してください。**Approve all** が有効な場合、別途確認が求められるとは限りません。

`space-quiz` の Copilot アプリセッションから、デプロイワークフローの準備を依頼します。

1. 次のプロンプトを送信します。

   ```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
   ```

## ワークフローとプルリクエストをレビューする

次の手順では、マージを許可する前に、ワークフローの設定と変更内容が意図どおりであることを確認します。

1. README がまだない場合に限って追加されていることを確認します。
2. プルリクエストの変更を確認します。ワークフローのビルドとデプロイの手順、トリガー、権限もレビューします。
3. ワークフローが未マージのプルリクエストではなく、既定のブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新後の差分をレビューします。マージを許可する前に、必須のプルリクエストチェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送ると、レビュー済みプルリクエストのマージ、リポジトリの Pages 設定変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作に別途確認が表示されない場合があります。

レビュー済みプルリクエストだけを対象にしてマージとデプロイを許可するため、次の手順を実行します。

1. `<PR_NUMBER>` をレビューしたプルリクエストの番号に置き換えてから、次のプロンプトを別に送信します。

   ```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![Space Quiz のプルリクエストのマージ、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の実行、yourname.github.io/space-quiz/ で公開された Space Quiz を示す3段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

デプロイが成功し、公開サイトが読み込まれることを確認します。

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

3. エージェントが Pages を有効にしたりプルリクエストをマージしたりできない場合は、案内された具体的な手順に従います。デプロイが成功し、公開サイトが読み込まれるまでは完了とみなさないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告だけに頼らずデプロイを確認できました。[レッスン 13: レビューと次のステップ][next-lesson]に進みましょう。

[next-lesson]: ../13-review/
