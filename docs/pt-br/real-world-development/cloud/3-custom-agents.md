---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

[Agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios do seu fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções específicas, práticas recomendadas, padrões de código e conhecimento de domínio. As equipes podem codificar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar tarefas. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes têm uma janela de contexto própria e podem orquestrar skills (e até outros agentes). Neste laboratório, o agente de acessibilidade revisa e atualiza o site conforme as diretrizes de acessibilidade; ele poderia chamar uma skill de fluxo de trabalho de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única maneira certa de criar um agente personalizado. Teste e itere para descobrir o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com os agentes personalizados, você aprenderá:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro por causa do contraste insuficiente entre texto e plano de fundo. Para resolver esse problema, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar a funcionalidade rapidamente.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e abra o arquivo do agente:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - erros comuns a evitar.

## Criar e atribuir uma issue

O painel de agentes é a central para trabalhar com agentes no seu ambiente. Nele, você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e redirecionar a sessão ou fornecer orientação adicional. Vamos pedir ao Copilot que crie o modo de alto contraste.

1. Acesse o repositório e selecione a guia **Issues**.
2. Selecione **New issue** e, em seguida, **Blank issue**.
3. Defina **Title** como `Add high contrast mode to website`.
4. Em **Description**, informe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

5. Selecione **Create** para criar a issue.
6. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
7. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Atribuição do agente de nuvem com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

8. Selecione **Assign**. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Este exercício apresentou os [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios específicos. Eles permitem transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] pelo painel de agentes.

## Recursos

- [Sobre os agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
