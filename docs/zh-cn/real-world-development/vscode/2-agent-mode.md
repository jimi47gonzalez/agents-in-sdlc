---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再次修改……这类工作往往会不断重复。

Copilot Agent Mode 可以帮助你处理这类任务。

Copilot Agent Mode 在 IDE 中自主工作，类似于开发者：先探索现有项目结构，再完成必要的更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> Agent Mode 和 cloud agent 的名称相似，但面向不同的使用场景。Agent Mode 在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot Agent Mode 探索项目、查找相关文件并协调多处更改。
- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现功能。
- 在将代码合并到代码库前，审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习已添加 publishers 助手函数，现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在更改代码前，先浏览 Tailspin Toys 网站，了解它现有的功能。

这个网站是带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表和详情。网站是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面在 frontmatter 中直接通过 Drizzle ORM 查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问助手函数位于 `src/lib/`，交互功能则通过范围有限、使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前运行网站的 codespace 的私有隧道。

### 浏览网站

网站启动后，可以浏览现有功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后，会打开包含更多信息的详情页，例如标题、说明、发行商和类别。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用的模式和模型可能不同，步骤会在需要时说明。

网站的初始版本可以正常使用，但还需要增加功能。先查看项目待办列表。使用模板设置项目时，系统已自动创建一组待处理议题；让 GitHub Copilot 帮你查看它们。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此课程中列出了该模型，希望由此获得相对一致的体验。不过，LLM 的输出具有概率性，你收到的建议可能与课程示例不同。这是正常现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示词，让 Copilot 查看议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令以列出所有议题。
5. 查看生成的议题列表。

注意，Copilot 还根据它判断对用户最有用的内容，为这些事项排定了优先级。

## 查看指令文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用 [user interface (UI)][ui-instructions] 文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指导，包括：
   - 应用架构概览。
   - 组件设计、可测试性和无障碍体验原则。
   - 指向特定文件类型指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在目录。这样可以复用内容，将较大的指令拆分成更小、更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用做三项更新：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还需要运行并通过测试。Copilot Agent Mode 可以完成这些任务，接下来让它实现筛选功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择了 **Agent**。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据待办列表中相关议题的要求实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与功能相关的文件。它应找到数据层助手函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，这可能需要几分钟。在此过程中，编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 按钮，因此无需在工作过程中立即选择。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认是否运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败，这没有问题。Copilot 会反复生成代码并运行测试，直到完成任务且未检测到错误。

   ![Copilot Agent Mode 完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你学习了如何：

- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能。
- 让 Copilot Agent Mode 探索项目、查找相关文件并协调多处更改。
- 在将代码合并到代码库前，审查 Copilot Agent Mode 生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分练习：实现分页

随着游戏列表增长，网站需要支持分页。运用本练习所学知识，让 Copilot 为网站添加分页功能。编写提示词时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页条目数，还是使用固定数量。
- 提供足够的指导，让 Copilot 按照预期实现功能。
- 可能需要与 GitHub Copilot 迭代，要求它修改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：各有什么作用，以及何时使用][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
