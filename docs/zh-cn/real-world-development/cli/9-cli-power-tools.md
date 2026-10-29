---
title: "第 9 课 - 探索 GitHub Copilot CLI 斜杠命令"
description: "使用斜杠命令管理上下文、选择模型、共享会话，并了解可选的云端委派功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令提供高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来看看其他一些实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和 cloud agent 委派的可选命令。

## 场景

核心 CLI 工作流已完成。现在看看一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace；如果尚未打开，请从存储库根目录启动 Copilot CLI。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意当前模型、token 使用情况，以及上下文在系统指令、工具、消息和剩余空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时间时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有各自偏好。Copilot CLI 可以列出可用模型，并让你选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验有助于提升所有人使用 AI 工具的能力。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果想共享会话，请选择适合的目标，例如 `/share file` 可导出本地 Markdown 文件。
3. 将导出内容发送给其他人或发布前先进行审查。会话导出内容可能包含提示词、回复和项目详情。

发布链接或 gist 是可选的。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还为较大的任务提供了以下命令：

- `/fleet`：将相互独立的子任务分配给多个子智能体并行执行。
- `/worktree`：为单独任务创建隔离的 Git 工作树。
- `/delegate`：将任务发送给异步工作的 Copilot cloud agent；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程任务。尝试前先开始一个新的、范围明确的任务，并按照常规工作流审查结果。要进一步了解异步智能体工作，请继续学习 [cloud agent 研讨会][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 斜杠命令可用于配置 CLI、共享会话和查看幕后工作。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和 cloud agent 委派的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索。现在通过[回顾所学内容][next-lesson]并了解后续步骤，结束本次学习。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
