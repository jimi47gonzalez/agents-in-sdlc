---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nessa central, você vê as tarefas com pull requests (PRs) abertos, explora as operações executadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe decidiu que também seria útil adicionar um modo claro. Como o trabalho já atualiza o estilo do site e adiciona alternadores, parece lógico incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente ambos os modos.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o estado das tarefas atribuídas ao agente de nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas nos painéis principal e esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, como:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, como:
   - obter o código do repositório.
   - criar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas variam conforme o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que adicione o alternador de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à implementação do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às exibidas no exemplo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem com a caixa de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot atualizará a tarefa. Ele incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso pode levar alguns minutos. Aproveite a pausa para refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, a central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento.

Você explorou:

- como acompanhar tarefas na página de agentes.
- como redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
