---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的开发流程，帮助我们以正确方式构建正确功能。本课开始一系列课程，沿用典型的智能体驱动开发流程：从议题开始生成新功能、确认代码有效且功能符合预期，最后成功合并。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。实际工作中，通常会为不同文件类型使用不同分支或 PR；本研讨会为聚焦核心概念而简化了流程。

在本课中，你将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现该功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办议题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则，反而使这些原则更加重要。多数开发者的流程大致如下：

1. 从描述需求的已创建议题开始。
2. 制定构建计划。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 不同团队和组织的具体流程会有所不同，但大多与上述流程相似。

遵循这种标准流程，可确保 AI 生成的代码满足需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**决定代理拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并迭代，直至任务完成。

先进入 Plan 模式并审查计划，然后再使用 Autopilot 实现。

## 从议题开始

开始筛选工作前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划让你在交由 Copilot 执行前，先确定功能实现或任务处理方案。对于复杂工作，花些时间制定计划通常很有帮助。切换到计划模式并要求 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定最佳方案。

3. Copilot 可能会询问筛选功能的行为，例如类别组合方式。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 要求 Copilot 修改任何不符合你决定的计划内容。

## 批准 Autopilot

计划已编写并审查，现在可以让 Copilot 实现。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与议题以及规划时做出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查的输出，并要求 Copilot 解决任何失败。

## 体验新功能

代码看起来不错，但还要确认它确实可以运行。像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。在本课中，你：

- 从 GitHub 议题开始了新的 Copilot 对话。
- 在 Plan 模式中定义了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义指令][next-lesson]深入了解如何让代码遵循书面规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
