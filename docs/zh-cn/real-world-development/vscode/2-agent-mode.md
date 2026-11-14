---
title: "练习 2 - 使用 Copilot Agent Mode 添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者通常需要找到所有相关文件、进行修改、运行测试、调试、发现遗漏的文件，再继续修改。这个过程往往会反复进行。

Copilot Agent Mode 可以帮助处理这些工作。

Copilot Agent Mode 可在集成开发环境 (IDE) 中更自主地工作。它会先探索项目结构，执行所需更新、运行测试等任务，并自动修复发现的问题。现在使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> Agent Mode 和云智能体名称相似，但适用于不同场景。Agent Mode 在 IDE 中执行任务，适合快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调更改。
- GitHub Copilot Agent Mode 如何跨用户界面和数据层实现新功能。
- 合并前如何审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成剩余的数据层、界面和测试工作。

## 运行 Tailspin Toys 网站

修改代码前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态超文本标记语言 (HTML)。页面通过 Drizzle 对象关系映射 (ORM) 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过小型、限定范围的 Astro `<script>` 和标准文档对象模型 (DOM) API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站。

1. 返回 Codespace。上一练习后应已位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 更新主分支并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，从终端选择 localhost URL 会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，此时网站服务器正在其中运行。

### 浏览网站

网站运行后，可以查看其功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习的模式和模型选择不同，相关步骤会在需要时说明。

网站初始版本可以正常运行，但还需要增加功能。先查看待办事项。使用项目模板时会自动创建一组议题，请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此说明中列出了该模型，希望由此获得相对一致的体验。由于大型语言模型具有概率性，实际建议可能与文档不同，这是正常且预期的情况。

> [!NOTE]
> Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还根据它判断对用户最有用的程度，为议题排序。

## 审查指令文件

让代理生成代码前，先审查用于向 Copilot 提供上下文的指令文件。项目中的[用户界面 (UI)][ui-instructions]指令文件说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体开发指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括 Astro、Tailwind 层叠样式表 (CSS) 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分较大的指令集，并为 Copilot 提供示例。

## 实现筛选功能

完成筛选至少需要对应用进行三项更新：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页以引入筛选界面。

此外，合并前必须运行并通过测试。Copilot Agent Mode 可以完成这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选择 **Agent**。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件，包括数据层辅助函数、界面和测试；随后它会修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行探索项目、修改文件和运行测试等任务。根据任务和代码库复杂度，这可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。工作完成后，每项更改都会有 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格请求确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这是正常的。Copilot 会在代码生成和测试之间迭代，直至任务完成且未发现错误。

   ![Copilot Agent Mode 完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他人工智能 (AI) 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

你已使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加功能。你了解了：

- GitHub Copilot Agent Mode 如何跨用户界面和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、识别相关文件并协调更改。
- 合并前如何审查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索：实现分页

随着游戏列表增加，网站需要分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示时，提供 Copilot 所需指导，以便按预期实现功能。
- 你可能需要与 GitHub Copilot 多轮迭代，要求它修改并提供上下文。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能与适用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
