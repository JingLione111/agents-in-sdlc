---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你要求 Copilot 云智能体执行了三项改善用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务、了解它执行的操作，甚至引导进行中的会话。

在本课中，你将：

- 探索智能体页面并监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到也可以一并添加浅色模式。由于已在更新网站样式并添加切换功能，新增此模式较为合理。现在引导智能体同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 为游戏生成 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的步骤，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他操作。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方案而变化。

4. 查看右侧显示的拉取请求面板，其中可以看到 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

了解当前任务后，现在要求 Copilot 在处理高对比度模式时，也添加浅色模式切换开关。

1. 选择实现高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示引导活动会话对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已回应，并将新请求加入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，再将新请求纳入后续工作。

这通常需要几分钟。等待时，可以回顾并思考目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心中，可以跟踪所有正在进行和已完成的任务、审查工作，也可以重定向进行中的任务来调整范围或提供额外指导。

你探索了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话以请求添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
