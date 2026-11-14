---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行代码更新和功能开发等任务。完成后，它会创建草稿拉取请求，供开发者审查。这样可以将繁琐任务交给代理处理，加快开发流程，让开发者专注于更重要的工作。

你将探索云智能体的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中所有导出的函数添加 TypeScript 文档 (TSDoc) 注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页中显示同类别的其他游戏，方便用户继续浏览。当前不需要完善的实现，只需支持用户体验验收测试即可。该工作目前受阻，且优先级低于其他议题。

这些都是容易被延后的任务，适合交给 Copilot 云智能体异步处理。开发者可先处理其他工作，之后再审查 Copilot 的成果并确认符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法，使用它并不能跳过思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请谨慎[界定云智能体任务范围][cloud-agent-best-practices]。将它视作人工智能 (AI) 结对程序员，分阶段工作、学习、试验并逐步调整。生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已经完成自定义指令实践，你已见过 `.github/copilot-instructions.md` 的作用；否则，可以先快速查看。分配任务前，以只读方式检查存储库中已有的指令文件，稍后可观察它们的影响。

在 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：检查 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定到 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。稍后的 PR 审查练习还会再次检查这些内容。

## 设置云智能体开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本。将任务分配给以类似软件工程师方式工作的 Copilot 时也一样。

云智能体使用 [GitHub Actions][github-actions] 环境执行工作。可以在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在任务开始前运行，以便提供必要的开发工具和依赖项。项目已预先配置此工作流，以便顺利完成实验。它会为单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需浏览器，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为步骤所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流文件类似，但有两点值得注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，会在 GitHub Actions 中运行此作业。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流，以便验证它是否成功，而不必等待 Copilot 启动。

## 添加文档注释

虽然大家都知道文档的重要性，但许多项目的文档已经过期或根本缺失。这类技术债务常常无人处理，会降低效率，也让维护代码库或引入新开发者更困难。Copilot 擅长生成文档，因此适合将此议题交给云智能体。它会在后台生成文档，之后可审查其工作。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办列表，因为它正在创建环境（运行前文提到的工作流）、分析项目并确定处理方案。

13. 审查待办列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并确定方案，以及生成代码。

整个过程可能需要几分钟。云智能体的主要用途之一是异步执行任务，让开发者可以专注于其他工作。趁它处理任务时，继续让云智能体处理另一项工作，并着手编写应用功能。

## 为游戏详情页添加相关游戏

GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注一类任务，代理处理另一类任务。为设计团队添加相关游戏区域可能不会花很长时间，但仍可将这段时间用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二项请求。它的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在可以转向创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、新功能或框架迁移等任务。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台工作时，现在可以创建并使用自定义智能体。[云智能体还可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并可应用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
