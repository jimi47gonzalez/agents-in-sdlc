---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义设置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，随后自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并使用它在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 已交付的内容

本研讨会有两个 PR 里程碑，每个 PR 都从已更新的 `main` 分支创建：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的“尚无评分”状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为了简化研讨会流程，我们将这些工作合并为一个 PR。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、手动浏览器检查，以及 Copilot 使用 MCP 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按你新设定的格式报告结果。创建 PR 前，QA 智能体结合这些结果、需求审查和测试覆盖范围进行了检查。

新增测试应解决实际覆盖缺口；如果 QA 检查后认为无需新增测试，也可能是合理结果。授权合并前，请审查代码和证据；更改后应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置文件并配置 MCP 服务器。这些自定义设置可以在多个对话中复用，也可以根据团队需求调整。指令用于设定标准，技能用于描述可重复任务，自定义智能体用于定义专业角色，MCP 服务器则连接外部工具。请检查实际更改和工具结果，而不只是阅读智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑实施方案，使用 **Interactive** 对专注的小型更改保持参与，使用 **Autopilot** 执行范围明确的任务。常规编辑可以选择速度更快的模型，复杂任务则可以选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 更多可探索的功能

核心工作流已经完成。还可以了解一些 Copilot CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取不同视角。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最好方式就是持续使用它。可以将其用于生产代码、个人项目，或者多年一直想做却尚未开始的小应用。与团队分享经验，也向团队学习。一如既往，还要继续查阅文档。

如果想继续从终端扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
