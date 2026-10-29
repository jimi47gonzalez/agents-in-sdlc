---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para implementar a filtragem de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto, mas alterações maiores exigem um processo mais robusto. O GitHub Copilot CLI foi criado para trabalhar com o fluxo existente da organização e ajudar a construir as soluções certas da maneira adequada. Nesta lição, você iniciará um processo comum de desenvolvimento orientado por agentes: usará uma issue para criar um recurso, verificará o código e o comportamento e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante o fluxo de desenvolvimento do recurso. Normalmente, haveria branches ou PRs diferentes para cada tipo de arquivo, mas neste workshop vamos simplificar para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar o recurso no modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar o trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como combinar categorias precisam ser definidos antes de programar. Use o modo Plan para tomar essas decisões e, depois, autorize uma implementação com escopo limitado no modo Autopilot.

## Contexto

Introduzir agentes de programação com IA não muda os fundamentos do desenvolvimento. A maioria das pessoas desenvolvedoras segue um processo semelhante:

1. Começar com uma issue registrada que descreva o trabalho.
2. Criar um plano do que precisa ser implementado.
3. Desenvolver e revisar o código.
4. Executar testes para validá-lo.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão e da aprovação da integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Seguir esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Começar pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o.
2. Confirme se o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para selecionar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` (ou um número semelhante). O caractere `#` adiciona o contexto de uma issue ou pull request do GitHub.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem antes de delegar a implementação. É recomendável planejar tarefas complexas. Vamos alternar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue, envie:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explorará o projeto e definirá uma abordagem.

3. Ele poderá fazer perguntas sobre o comportamento da filtragem. Responda com base em suas preferências.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise partes que não correspondam às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, implemente-o usando o Autopilot. Esse modo permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na sua versão.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot executar o plano, gerar código e rodar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Revise o código gerado antes do merge, como faria com qualquer código.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas no planejamento.
4. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Inicie o aplicativo e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros de categoria e editora, inclusive as combinações definidas no plano.
4. Confirme se a redefinição e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos aprofundar como o código é gerado e garantir que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
