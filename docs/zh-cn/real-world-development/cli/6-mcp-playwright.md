---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。我们还要处理数据和外部服务，并允许 Copilot 使用其他自动化工具。模型上下文协议 (MCP) 服务器可以让 Copilot CLI 使用内置功能之外的工具和服务。

在本课中，你将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚不可用，则添加 Playwright MCP 服务器。
- 请代理驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证界面更新，还需要实际与界面交互。你希望 Copilot 像用户一样操作正在开发的网站，从而自动检查更改是否符合预期。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为人工智能 (AI) 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息并代表你执行操作。

工具和资源通过 MCP 服务器访问。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个服务器代表智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并贡献生态系统资源。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 探索功能

之前你已手动确认功能行为。现在使用 Copilot 中的 Playwright MCP 服务器重复验证。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它往往会自行判断。但指出应使用的服务器有助于保持结果一致，也能减少一些令牌使用。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认行为符合预期。

Copilot 将启动服务器，通过 Playwright 操作网站，停止服务器并提供报告。

## 总结和后续步骤

你已使用 Playwright MCP 服务器，从 Copilot CLI 在真实浏览器中探索功能。回顾一下，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 尚不可用，则添加了服务器。
- 请代理驱动浏览器并探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合到专业角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
