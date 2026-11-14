---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用代理模式时，GitHub Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于工作会实时发生在你的计算机上，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以在聊天中查看差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行时引导智能体添加浅色模式切换开关。

## 场景

上一练习中使用的无障碍自定义智能体正在实现高对比度模式。工作进行时，设计团队又提出一个需求：为觉得默认主题太暗的用户添加浅色模式切换开关。不必从头开始新对话，可以引导现有会话扩展功能。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内嵌差异显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用，包括文件读取、文件编辑以及需要运行的终端命令。
3. 智能体提出文件编辑时，差异会以内嵌方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。完成后应会显示更改摘要。

## 引导智能体添加浅色模式

高对比度模式已完成，现在在同一对话中添加浅色模式切换开关。继续使用同一聊天会话，智能体可以保留之前的上下文，包括已修改的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续之前的工作，探索已做的更改并提出浅色模式的修改。
3. 与高对比度模式一样，逐项审查新更改，并选择 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期，例如试图以不希望的方式重构切换界面，可选择聊天输入框旁的 **Stop** 按钮停止，然后发送澄清提示。

> [!TIP]
> 引导进行中的会话比重新开始更快。可以用后续提示要求调整（“简化层叠样式表 (CSS)，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查完整的已更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并查看差异。确认其中体现自定义智能体的无障碍实践，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导了正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本课中，你探索了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示在会话进行时引导智能体。

接下来，在[后续课程][next-lesson]中发布无障碍更改为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
