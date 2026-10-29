---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma envolver vários arquivos e operações, como executar testes. O fluxo típico inclui localizar os arquivos, fazer as alterações, executar testes, depurar, descobrir o que ficou faltando e atualizar novamente. É nesse cenário que o modo de agente do Copilot é útil.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente problemas encontrados. Vamos usá-lo para adicionar funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem atua de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente implementa recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras; agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para conhecer a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. As pessoas podem listar jogos e exibir detalhes. O site é um único aplicativo Astro que renderiza páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando Drizzle ORM; não há uma API de backend separada nem framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` Astro usando APIs DOM padrão.

### Iniciar o site

Uma tarefa `npm run dev` inicia o servidor de desenvolvimento Astro. No GitHub Codespace:

1. Volte ao codespace. Você deverá estar em `main` após o exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, explore os principais recursos:

- **Página inicial:** lista os jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo:** ao selecionar um jogo, você verá mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando necessário.

A implementação inicial do site funciona, mas queremos acrescentar recursos. Vamos começar examinando o backlog. A configuração do projeto com o modelo criou automaticamente um conjunto de issues; peça ao GitHub Copilot para mostrá-las.

1. Selecione **Agent** no menu suspenso de agentes da exibição Chat. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores não estão recomendando um modelo específico. Este workshop foi criado com Claude Sonnet 4.5 para que as sugestões fossem relativamente consistentes. Como os LLMs são probabilísticos, as sugestões podem variar; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie ao Copilot este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista criada. Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções de tipos de arquivo, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, o que permite reutilizar e dividir conjuntos grandes em arquivos menores e incluir exemplos ou modelos.

## Implementar a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Os testes também devem ser aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** está selecionado no menu suspenso de agentes.

   ![Captura de tela mostrando o seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado.
5. Envie este prompt para implementar a funcionalidade conforme a issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos da camada de dados, da interface e dos testes. Em seguida, ele modificará os arquivos e executará testes.

   ![Captura de tela do Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> A exploração, as alterações e os testes podem levar alguns minutos. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá esses controles para todas as alterações; não é necessário selecioná-los enquanto o trabalho está em andamento.

7. Quando solicitado, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. O Copilot alternará entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão completa do modo de agente no Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- implementar recursos na interface e na camada de dados.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes antes do merge.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra — Implementar paginação

À medida que a lista de jogos crescer, será necessário implementar paginação. Use as habilidades deste exercício e peça ao Copilot para atualizar o site. Considere:

- seguir as práticas existentes e usar os arquivos de instruções.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Copilot ask, edit e modos de agente: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
