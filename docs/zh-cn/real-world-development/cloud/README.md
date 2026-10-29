---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) 可在云端异步处理工作。你可以在 GitHub 上分配任务，cloud agent 会在后台执行：探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

本系列练习将先添加 cloud agent 会遵循的自定义指令，然后创建 GitHub 议题并让它实现任务。你还将审查并使用自定义智能体来影响它的工作方式，从智能体页面监控和引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 内容 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud agent][ex2] | 异步智能体 | 将议题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 查看并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 有 GitHub 帐户，并订阅有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为你的帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？符合条件的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会所需的智能体、MCP、代码审查和 Copilot CLI 功能，可用于完成所有环境中的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，Copilot Student、Pro、Pro+、Business 和 Enterprise 计划均提供 cloud agent。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本系列练习不涉及 MCP。要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 练习][cli]或 [VS Code 练习][vscode]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
