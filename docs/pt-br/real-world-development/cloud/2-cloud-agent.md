---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas de dívida técnica. Pode haver problemas de segurança sem solução, código legado que precisa de atualizações ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele gera um PR de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar o site deixaram a documentação em um estado inadequado: ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Ainda não é necessário um acabamento final; basta algo que permita testar a experiência. Embora essa tarefa esteja bloqueada no momento, há issues mais prioritárias.

Esses são exemplos de tarefas que podem perder prioridade e são boas candidatas para o agente de nuvem. Ele trabalha de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras tarefas e depois revise o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Uma das maneiras de iniciar o trabalho é [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão durante ou depois da execução. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não faz mágica: ainda é preciso pensar. Mesmo tarefas aparentemente simples podem ter complexidade. Por isso, [defina com cuidado o escopo do trabalho para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

As lições anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções deste repositório para reconhecer o efeito deles mais adiante.

Abra estes arquivos na interface Web do GitHub ou em um codespace:

- `.github/copilot-instructions.md`: examine **Code standards**, especialmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita essas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse tema ao revisar os PRs.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Isso disponibiliza ferramentas e dependências necessárias. O laboratório já configura esse workflow para instalar Node.js e dependências, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros workflows do GitHub, mas vale observar alguns detalhes:

- Há um único job chamado `copilot-setup-steps`. Ele é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar o Copilot iniciá-la.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção e a integração de novas pessoas à equipe. O Copilot é útil para criar documentação, por isso esta é uma issue adequada para o agente de nuvem. Ele trabalhará em segundo plano e, em uma lição futura, você revisará o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Em **Description**, informe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Aguarde o Copilot criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que o Copilot executará.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Percorra a sessão ao vivo para acompanhar como o Copilot resolve o problema. Você verá a exploração do código e do estado do projeto, as pausas para analisar e decidir a abordagem e a criação do código.

Isso provavelmente levará alguns minutos. Um dos objetivos principais do agente de nuvem é executar tarefas de forma assíncrona e permitir que você se dedique a outros trabalhos. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, trabalhando em funcionalidades do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é dividir o trabalho: enquanto você se concentra em uma tarefa, ele pode executar outra. Vamos atribuir ao Copilot a solicitação da equipe de design.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Em **Description**, informe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, você verá 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O emoji de olhos indica que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o trabalho quando terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao agente para execução assíncrona e usá-lo para resolver dívida técnica, criar recursos ou migrar código entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir operações seguras.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, apresentadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
