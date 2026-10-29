---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些内容。[指令文件][instruction-files]可以提供此类指导，让 Copilot 不仅了解你要完成什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码规范和文档标准。
- 在现有指令下生成用于筛选的第一段数据逻辑（publishers 助手函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 使用相同提示词重新生成代码，观察新标准如何影响结果。
- 将指令更新和筛选代码提交到 `main`，供 Copilot 在下一练习中使用。

> [!CAUTION]
> 生成的代码可能不符合你设定的所有标准，因为 Copilot 具有非确定性。本练习旨在观察更新指令后行为上的整体变化，而不是要求输出逐字相同。

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

## 为更改创建分支

按照最佳实践，为本次更改创建一个分支。

1. 返回上一个练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用的模式和模型可能不同，步骤会在需要时说明。

要观察自定义指令的影响，先在当前指令文件下发送提示词。稍后更新指令后，再发送相同提示词进行比较。

1. 关闭前面练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解助手函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改助手函数文件及其测试。
6. 留意生成的助手函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释标头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系，本练习重点仍是观察更新指令后行为的一致性变化。

## 添加新的存储库标准

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

## 重新发送提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在生成的文件开头会出现类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中。下一练习将以此为基础继续开发。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用源代码管理。接下来，将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认指令和助手函数更改均已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为助手函数基础代码所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并关联待办列表中的文档标准议题。也可以让 Copilot 使用已连接到项目的 GitHub 工具代为完成。向 Copilot 提示查找议题、创建并关闭该议题的拉取请求，然后合并请求。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已为工作区配置 GitHub 工具，因此无需另行设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示完成操作即可。后续练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建拉取请求，说明其中既有指令更新，也有为后续筛选工作准备的 publishers 助手函数基础代码：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建拉取请求。
5. 当 Copilot 提示你**允许**它代表你执行 GitHub 操作时，请先检查操作内容，再酌情选择 **Allow**。
6. 创建拉取请求后，提示 Copilot 合并请求并将分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，请先检查命令，再酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何读取指令文件中的上下文，并在 VS Code 中使用 Copilot Chat 完成了以下操作：

- 发送代码生成提示词，观察 Copilot 如何根据**现有**指令生成内容。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 重新发送相同提示词，观察生成的代码如何遵循新标准。
- 将指令更新和助手函数基础代码提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更出色 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
