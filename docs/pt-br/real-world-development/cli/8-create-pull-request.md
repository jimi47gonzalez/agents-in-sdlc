---
title: "Lição 8 - Criar e mesclar o PR do recurso"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes em conjunto; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você fez o merge do PR das avaliações por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em vários PRs. Para simplificar o workshop, todo o fluxo de filtragem e qualidade ficou em uma conversa e branch e será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar a alteração completa e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar o merge.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Depois de habilitá-lo, o Copilot resolve os bloqueios do PR — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e, então, habilita o merge automático do GitHub se o repositório permitir.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitá-lo.

## Usar o Agent Merge para gerenciar o PR

Com o código pronto, revise-o, crie o PR e permita que o Agent Merge conduza as etapas restantes.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agente com `/agent`.
3. Selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um PR com `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua no PR. Quando as verificações forem aprovadas, fará o merge.
7. Confirme se o Copilot informou algo semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo e faça manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento: geração, teste e validação de código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou a alteração completa e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge após a revisão e confirmou o merge.

Em seguida, você [explorará outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
