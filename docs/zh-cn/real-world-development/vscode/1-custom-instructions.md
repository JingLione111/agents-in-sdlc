---
title: "练习 1 - 自定义指令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导代码生成。

在本练习中，你将：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不会完全遵循你设定的标准。Copilot 具有非确定性。本练习旨在观察更新指令后行为的整体变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代风格。
- 应使用 TypeScript 文档 (TSDoc) 注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 按照这些实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码规范、库和注释习惯等偏好，为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次存储库请求都会发送给 Copilot 的单个指令文件，应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本练习不会全面介绍如何创建指令文件，但示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例。

创建指令文件没有唯一方法，使用 AI 也是如此。通过实践可以找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件集合。浏览本项目文件时，可以看到适用于[用户界面更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应练习会在相关处说明。
>
> 如需模板或起点，可查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读存储库自带的指令文件，包括一个核心 `copilot-instructions.md` 和多个 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些指导适用于 Copilot 在此存储库中的交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目中创建单元测试的专用指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分成可复用文件，并为 Copilot 提供参考示例。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但尚未要求代码内文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

为遵循最佳实践，先为更改创建一个分支。

1. 返回上一练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

为观察自定义指令的影响，先在现有指令下发送提示。更新指令后，再次发送相同提示并比较结果。

1. 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异，相关步骤会在需要时说明。
2. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
3. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
4. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

5. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

6. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
7. 注意建议的辅助函数是带类型的函数，以 `db` 客户端为第一个参数，并返回带类型的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层规范。
8. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有相关指令，也可能生成文档注释。如果发生这种情况也没关系；本练习的重点仍是观察添加指令后行为的一致性改进。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。为改善代码建议，现在将编码标准写入文件。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。它说明了项目编码标准，但尚无代码内文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。存储库级文件适合存放刚添加的文档注释等项目级标准。

## 重新运行提示并观察变化

再次发送相同提示，以查看更新后的代码是否遵循新标准。

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意新生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习要继续使用的代码。

## 提交并推送指令分支

指令文件和存储库中的其他资源一样，也需要通过源代码管理。提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR，然后合并它。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在 Codespace 中打开 Copilot Chat，并确认代理选择器中已选择 **Agent**。
3. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图。
4. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

5. Copilot 开始查找议题并创建 PR 时，如果系统请求授权它代表你执行 GitHub 操作，请审查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统请求授权 Copilot 执行 GitHub 操作和 shell 命令，请审查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何获取项目指令文件中的上下文，并使用 VS Code 中的 Copilot Chat：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成了筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加了新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中使用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
