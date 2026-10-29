---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 合作完成了多项任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队实现相关游戏功能供其迭代，并实现包括高对比度和浅色模式切换开关在内的无障碍功能。接下来审查代码更改，必要时向 Copilot 提供反馈，改进其工作，然后为团队中的其他成员启用审查。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 针对代码文档、相关游戏功能和无障碍功能所提出的更改。

## GitHub Copilot cloud agent 的安全性

Copilot cloud agent 会在没有监督的情况下异步执行任务，因此必须设置一些安全限制：

- Copilot 只能读取存储库，并且只对用于编写代码的分支具有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR），也就是为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求（实际标题可能更具体）。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成任务。这可能需要几分钟，可以趁此休息一下或回顾之前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新代码中的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释标头。这些来自练习 2 开始时查看的自定义指令文件。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流进入拉取请求检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像 @ 提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理反馈。由于结果具有非确定性，无法给出固定的提示词。可以要求 Copilot：

- 在每个代码文件开头添加简短注释，说明文件的用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在评论中请求 Copilot 修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意它会启动新会话来应用更改。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，再合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 创建的、用于解决游戏详情页显示相关游戏问题的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR（实际标题可能更具体）。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流进入拉取请求检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经实现了相关游戏功能！与之前一样，可以通过迭代改进 Copilot cloud agent 的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释标头和 TSDoc 文档注释（请记住，分配此议题时，尚未更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中发表评论来提出更改，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。这个 PR 应该同时包含练习 3 中分配的高对比度模式，以及练习 4 在 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR（实际标题可能更具体）。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成任务，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，特别留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流进入拉取请求检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且页面重新加载后仍会保留设置。

注意，无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习：继续委派工作

Cloud agent 最适合处理待办列表中的真实任务，让你能专注于其他工作。可以在自己的存储库中再创建几个议题并分配给 Copilot，以养成这种工作方式。以下是一些建议：

- 在游戏详情页中创建支持者兴趣表单。
- 为游戏列表页添加分页功能。
- 为数据访问助手函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境的全部练习。在这些课程中，你：

- **检查了存储库随附的自定义指令文件**，以便稍后查看它们如何影响 cloud agent 的输出。
- **将议题分配给 Copilot cloud agent**，并观察它如何设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**，为无障碍体验添加高对比度和浅色模式切换开关。
- **将 Copilot agents 页面用作 mission control**，监控并引导无障碍会话。
- **审查并迭代改进 cloud agent 创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 环境的全部练习。其他环境中的研讨会可以补充本次实践：

- 🖥️ **[VS Code 环境](../../vscode/)**：在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)**：在终端中使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接，交付两项经过审查的更改。

还可以在自己的存储库中尝试以下内容：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
