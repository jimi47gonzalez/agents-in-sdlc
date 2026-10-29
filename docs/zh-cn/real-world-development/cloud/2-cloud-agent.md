---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以更新代码和添加功能。完成任务后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发进度，并让开发者专注于更重要的工作。

本练习将使用 Copilot cloud agent 学习如何：

- 自定义生成代码的环境。
- 确保安全地执行操作。
- 编写范围清晰的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下完善的文档，事实上可以说完全缺少文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每个游戏详情页上显示相关游戏，也就是同一类别中的其他游戏。目前不需要完善的实现，只需要能用于验收测试和体验评估的版本。此需求目前受阻，不过还有优先级更高的议题。

这两项任务都可能被不断推迟，适合交给 Copilot cloud agent 异步处理。开发者可以继续其他工作，之后再审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行时或完成后查看它的操作。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，不能替你思考。看似简单的操作也可能包含复杂环节，因此[应仔细限定 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已经完成了自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。将工作分配给 Copilot cloud agent 前，先只读查看本存储库中的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面中打开以下文件；如果已启动 codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给 cloud agent 后，留意拉取请求中的 TSDoc 文档注释、TypeScript 约定和注释标头。这些要求来自刚才查看的指令文件。后续练习会再次介绍如何审查拉取请求。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境，并运行一些设置脚本以确保环境正常。将任务分配给 Copilot 时也是如此，它会以类似 SWE 的方式处理任务。

Cloud agent 使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，以便 Copilot 开始工作前安装必要的开发工具和依赖项。为方便实验，项目已预先配置此工作流。它会为单个 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 其中的 `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这便于提前测试工作流是否成功，而不必等待 Copilot 启动。

## 添加文档

大家都知道文档很重要，但大多数项目的信息要么已过时，要么根本不存在。这种技术债务往往无人处理，降低工作效率，也增加维护代码库或帮助新开发者加入团队的难度。Copilot 很适合生成文档，因此可以把这项工作分配给 Copilot cloud agent。它会在后台生成所需文档，之后的练习会审查其成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办列表及 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口，才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的一个主要目标是异步执行任务，让我们能继续处理其他工作。接下来利用这一特点：将另一项任务分配给 Copilot cloud agent，同时编写代码为应用添加功能。

## 在游戏详情页中添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的优势之一是可以分配工作，让你专注于其他任务。即使为设计团队添加相关游戏部分不一定需要很长时间，也可以将时间用于其他工作。现在将这项任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，你应该会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！Copilot cloud agent 的工作方式类似 SWE，因此无需一直监控，完成后再审查即可。现在转而创建和使用自定义智能体。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 cloud agent，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本练习介绍了以下概念：

- 自定义生成代码的环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作。接下来将学习创建并使用自定义智能体。Copilot cloud agent 也可以使用 [MCP 服务器][cloud-agent-mcp]，并能读取之前介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
