---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra. Eles dão acesso a recursos avançados, informações de bastidores e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para conhecer os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora, vamos explorar recursos adicionais: gerenciar contexto, alternar modelos, compartilhar sessões e, opcionalmente, delegar trabalho ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas grandes ou complexas, talvez você alcance o limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário. Você também pode inspecionar ou compactar o contexto com comandos de barra.

1. Volte ao Codespace e, se ainda não estiver aberto, inicie o Copilot CLI na raiz do repositório.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. Talvez não haja uma grande diferença se a conversa ainda for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` para escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação e iniciar uma conversa limpa.

## Escolher um modelo

Modelos diferentes têm pontos fortes diferentes, e cada pessoa pode ter suas próprias preferências. O Copilot CLI permite listar e selecionar o modelo desejado.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a usar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar uma GitHub gist.

1. Digite `/help` e consulte as opções de `/share` na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não queira compartilhar.

## Opcional: ampliar o trabalho ou delegar

O workshop principal está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` divide subtarefas independentes entre subagentes e as executa em paralelo.
- `/worktree` cria uma worktree Git isolada para outra tarefa.
- `/delegate` envia uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Comece uma tarefa nova e bem delimitada antes de experimentá-los e revise os resultados pelo fluxo normal. Para explorar melhor o trabalho assíncrono, continue no [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra permitem configurar o Copilot CLI, compartilhar sessões e ver o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` exporta ou compartilha sessões.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há mais comandos de barra para explorar. Vamos encerrar a jornada [revisando o que aprendemos][next-lesson] e os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
