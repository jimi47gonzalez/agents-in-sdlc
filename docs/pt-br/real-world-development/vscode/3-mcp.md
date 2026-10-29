---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e peça ao Copilot que controle um navegador real. Depois, publique a branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que teste manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais útil quando consegue acessar recursos fora do editor. O Model Context Protocol (MCP) é o padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e suas interações com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas (por meio de recursos) e executar ações em seu nome (por meio de ferramentas).

Essas ferramentas e recursos são acessados por um servidor MCP, que faz a ponte entre o agente de IA e os serviços externos, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegadores com o Playwright, como acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub hospeda um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada vem do modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não há nada para instalar localmente. O VS Code conecta-se por HTTP, e você autentica-se no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicionar o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é local: o VS Code inicia-o na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário no codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que não há navegador, peça para executar `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: quando o Copilot precisa das ferramentas de um servidor pela primeira vez, o VS Code o inicia e pede confirmação de confiança.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo.

Iniciá-lo manualmente é opcional. Se você ignorar essa etapa, o VS Code o iniciará e pedirá confirmação quando o Copilot precisar das ferramentas no próximo passo.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. O agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. Assim, você verifica rapidamente o comportamento sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. O agente raciocina sobre elementos estruturados e rotulados (botões, links e itens de lista), como faz a tecnologia assistiva; por isso, uma verificação funcional rápida também serve como uma avaliação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas de acompanhamento ou peça a correção antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira utilização do servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub. Como você já está usando o GitHub Copilot, peça que ele crie o PR e associe a issue correspondente.

1. Abra o painel Copilot Chat e selecione **New Chat**.
2. Confirme se **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Envie este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Abra o link criado pelo Copilot para revisar o PR, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o MCP e como servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar a filtragem no navegador antes da entrega.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
