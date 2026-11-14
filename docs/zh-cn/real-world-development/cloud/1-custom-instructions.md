---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可向 Copilot 提供指导，让它了解你不仅要求它做什么，也要求它如何完成。

在本练习中，你将：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会比较更新指令前后的提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时对比。稍后审查云智能体创建的拉取请求时，可以观察指令文件的影响。

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

- `.github/copilot-instructions.md`：每次存储库请求都会发送给 Copilot 的单个指令文件，应包含适用于大多数请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
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

阅读存储库自带的指令文件，包括一个核心 `copilot-instructions.md` 和多个 `*.instructions.md` 文件。可在 GitHub 网页界面或 Codespace 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些指导适用于 Copilot 在此存储库中的交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目中创建单元测试的专用指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分成可复用文件，并为 Copilot 提供参考示例。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但尚未要求代码内文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来需要编辑一次：添加项目级规则，要求代码中使用 TSDoc 文档注释和文件级注释头。云智能体稍后处理本工作环境中的议题时会读取这些规则。

编辑前先创建分支。云智能体会读取议题目标分支中的指令，但仍要将本次更改单独提交。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。它说明了项目编码标准，但尚无代码内文档规则，这正是生成的辅助函数没有文档注释的原因。
4. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。存储库级文件适合存放刚添加的文档注释等项目级标准。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支开始工作，因此必须先将指令更新合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习分配议题时，云智能体即可读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何获取项目指令文件中的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续课程中，你可以在云智能体创建的拉取请求中观察该标准的效果。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
