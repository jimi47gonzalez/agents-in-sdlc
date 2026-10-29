---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, incluindo alternadores para os modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, enviar feedback ao Copilot.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, existem restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente de nuvem executa tarefas no GitHub Actions em um ambiente separado e efêmero.
- Um ser humano precisa aprovar workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem para adicionar documentação ao código. Você usará a interface padrão de PRs do GitHub.com.

> [!NOTE]
> O PR pode mostrar um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso a recursos externos, inclusive servidores MCP, é limitado por padrão. Se quiser, [personalize ou desabilite o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com e selecione a guia **Pull Requests**.
2. Abra o PR intitulado algo como **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a marca **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

3. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

4. Examine o código atualizado, que agora inclui comentários TSDoc e outra documentação. O conteúdo exato varia. Verifique os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas mudanças decorrem dos arquivos de instruções personalizados que você examinou no Exercício 2.
5. Volte à guia **Conversation** e role a página. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

6. Os workflows serão adicionados à seção de verificações do PR. Aguarde as verificações do projeto para o aplicativo Astro serem aprovadas; isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR, como faria com uma pessoa da equipe. O Copilot verá o comentário e iniciará outra sessão para atendê-lo. Como os resultados não são determinísticos, não há um prompt único. Você pode pedir, por exemplo, para:

- adicionar cabeçalhos de comentário no início dos arquivos de código, descrevendo sua finalidade.
- adicionar comentários TSDoc aos arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário ao PR pedindo uma alteração na documentação gerada e mencione **@copilot**. Use uma das sugestões acima ou peça outra melhoria.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Uma nova sessão será iniciada.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar, um novo commit aparecerá no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com e selecione **Pull Requests**.
2. Abra o PR com um título semelhante a **Show related games on the game details page**.
3. Selecione **Files changed** e revise o código gerado.
4. Se estiver satisfeito, volte à guia **Conversation** e role a página. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

5. Aguarde as verificações do aplicativo Astro serem aprovadas. Opcionalmente, você pode testar manualmente o recurso no codespace. No terminal, substitua `<branch-name>` pelo nome da branch criada pelo Copilot (por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Você pode iterar e pedir ajustes, como alterar a quantidade de jogos exibidos ou adicionar cabeçalhos e comentários TSDoc (lembre-se de que esta tarefa foi atribuída **antes** da atualização das instruções personalizadas). Adicione um comentário na guia **Conversation** para iniciar outra sessão do Copilot.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados com o agente personalizado. O PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com e selecione **Pull Requests**.
2. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a marca **[WIP]**. Aguarde alguns minutos até que o trabalho termine.

3. Selecione **Files changed** e revise o código, prestando atenção a:
   - componentes de interface para alternar entre modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de CSS ou estilo dos modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação pelo teclado.
   - código JavaScript/TypeScript que controla a alternância de modos.
4. Volte à guia **Conversation**. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

5. Aguarde as verificações do aplicativo Astro serem aprovadas. Opcionalmente, teste manualmente os recursos no codespace. No terminal, substitua `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

   Inicie o aplicativo e teste os alternadores de alto contraste e modo claro no navegador. Confirme se as preferências continuam aplicadas após recarregar a página.

O agente personalizado de acessibilidade ajudou o Copilot a implementar os recursos seguindo práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e peça as alterações.

## Exercício opcional — continue delegando

O agente de nuvem é útil para tarefas reais do backlog que você pode delegar. Para praticar, crie outras issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse para apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizados do repositório** para observar seus efeitos no trabalho do agente.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar alternadores dos modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como central de controle** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
