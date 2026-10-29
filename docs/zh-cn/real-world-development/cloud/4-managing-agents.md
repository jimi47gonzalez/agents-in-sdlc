---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前面几节练习中，你让 Copilot cloud agent 执行了三项任务，目标都是改进用户体验和添加功能。Cloud agent 虽然可以异步自主工作，但监控任务进度仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有拥有未关闭拉取请求的智能体任务，了解它们执行的操作，还可以引导正在进行的会话。

本练习将学习如何：

- 使用智能体页面监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以顺便添加浅色模式。由于智能体已经在修改网站样式并添加切换功能，因此增加此功能也很合理。现在需要引导智能体，让它在高对比度模式之外再添加浅色模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作进度和采用的方法而异。

4. 同时留意右侧显示的拉取请求（PR）面板。你可以在这里查看 PR 和更改文件，进一步监控进度。

## 引导 cloud agent

查看正在进行的任务后，让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择用于添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。你会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents 页面中的任务屏幕截图，其中高亮显示 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认提示词，并将其纳入工作计划。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这可能还需要几分钟。可以利用这段时间暂停一下，回顾目前学到和体验的内容。

## 总结和后续步骤

本练习介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。借助此 mission control，你可以跟踪正在进行和已完成的任务、审查已执行的工作，也可以引导进行中的任务以调整范围或补充说明。

本练习介绍了以下概念：

- 使用 mission control 和智能体页面监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
