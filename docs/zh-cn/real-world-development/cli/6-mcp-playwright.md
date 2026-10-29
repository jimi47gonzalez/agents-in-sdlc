---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。还需要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以让 Copilot CLI 超越内置功能，访问更多工具和服务。

本课将学习如何：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加该服务器。
- 让智能体控制浏览器，体验筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，仍需实际与 UI 交互。你希望 Copilot 像用户一样操作开发中的网站，进一步自动验证更改，从而提高对功能正确性的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助用户发现并贡献生态系统内容。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用前应审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请将其添加，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

此前你已经手动确认功能正常。现在使用 Copilot 中可用的 Playwright MCP 服务器完成同样的操作。

1. 使用以下提示词，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。不过，既然知道该使用什么工具，明确指出也没有坏处。这有助于获得更一致的结果，还能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的各项步骤，以确认功能正常。
3. 阅读报告，确认一切行为符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你使用 Copilot CLI 中的 Playwright MCP 服务器在真实浏览器中体验了功能。本课中，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加了该服务器。
- 让智能体控制浏览器，体验筛选功能。

接下来[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
