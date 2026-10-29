---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些内容。[指令文件][instruction-files]可以提供此类指导，让 Copilot 不仅了解你要完成什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码规范和文档标准。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，这里不会运行前后对比提示词。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时并排展示效果。之后在本环境中审查 cloud agent 创建的拉取请求时，你会看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件的用途。

使用指令文件可确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码约定、库以及希望使用的注释类型，从而获得更相关的建议和代码片段。指令既可以适用于整个存储库，也可以仅适用于特定文件类型或任务。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。此文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：适用于特定任务或文件类型的指令文件。你可以针对特定语言（如 TypeScript）或任务（如创建 UI 组件或单元测试）编写指令。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不会全面讲解如何编写指令文件，但示例项目展示了一种有代表性的方法。总体来说：

- 将 `copilot-instructions.md` 中的指令限定在项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，清晰地描述要求，并给出正确和错误示例。

指令文件没有唯一的正确写法，AI 的使用方式也是如此。通过试验，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目中的文件时，你会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。各使用界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应的课程会在适当位置介绍这些功能。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中汇集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间查看此存储库随附的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob，用于匹配适用的文件。这里会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意针对单元测试创建方式的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并查看文件底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令拆分为较小、可复用的部分，并让 Copilot 参考具体示例。（这些路径相对于指令文件所在目录，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求为代码添加文档注释。接下来会添加 TSDoc 文档注释和文件注释标头的规则。

## 添加新的存储库标准

接下来要进行本课中唯一的编辑：规定应使用 TSDoc 文档注释和文件级注释标头为代码添加文档。之后，cloud agent 会在本环境后续练习处理议题时使用此要求。

在编辑前，先创建一个分支。（Cloud agent 会读取议题目标分支中的指令，但仍要将更改整洁地提交。）

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来，为存储库编码标准补充文档要求，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。此处记录了项目编码标准，但尚未规定代码文档要求，这也是生成的助手函数没有注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上节所见，指令文件既可以放在存储库级的 `.github/copilot-instructions.md` 中，为整个项目提供指导；也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。项目级标准（例如文档注释规则）适合放在存储库级文件中。

## 提交、推送并合并指令更新

Cloud agent 会读取议题目标分支中的指令。下一练习将把一个议题分配给 Copilot，而它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。之后 cloud agent 处理本环境中的议题时就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上完成本研讨会，可以跳过创建分支的步骤，并直接提交到 `main`。此处使用分支是为了演示真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何读取指令文件中的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来几节会看到 cloud agent 创建的拉取请求如何遵循该标准。

接下来将第一个议题[分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更出色 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
