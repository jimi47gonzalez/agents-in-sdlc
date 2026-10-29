---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认功能在浏览器中能够正常运行。无需手动浏览应用，你可以连接 **Playwright MCP 服务器**，让 Copilot 控制真实浏览器测试功能，然后发布分支并创建拉取请求。

本练习将学习如何：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当智能体能够访问编辑器以外的工具时，Agent Mode 会更加强大。MCP 是 Copilot 实现这种能力的方式：它为智能体与外部工具和服务通信提供了标准。

![智能体模式内部工作原理示意图，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，从而获取最新信息（使用资源）并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，并负责管理双方之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可用于访问工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并贡献生态系统内容。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并考虑安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

该 `github` 条目随项目模板提供，因此上一练习中 Copilot 才能读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 通过 HTTP 连接服务器，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来再添加一个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，用于测试刚实现的功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程的 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在后台运行浏览器，不显示可视窗口。由于 codespace 没有可显示窗口的桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要使用服务器工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请先审查配置，再选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用服务器工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 控制真实浏览器。无需手动浏览应用检查工作，智能体可以打开页面、导航、应用筛选条件、读取结果并向你总结。这样无需离开当前对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器会读取页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体处理的是结构化且带标签的元素（如按钮、链接和列表项），与辅助技术的工作方式相同。这意味着简单的功能检查也能顺便验证基本的无障碍体验。

连接服务器并启动应用后，发送以下提示词，让 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并执行这些步骤，然后报告结果。请根据议题中的验收标准审阅它的总结。如果发现问题，可以继续提问或让 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请重新启动后再发送提示词。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）等方式创建拉取请求。也可以让 GitHub Copilot 帮忙：它可以查找相关议题，并创建与之关联的拉取请求。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 创建拉取请求：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证，请按照提示操作。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建拉取请求。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用它在浏览器中手动测试筛选功能。
- 发布了分支，并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来创建[自定义智能体][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
