---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa deve ser feita de uma maneira específica — ou há informações de contexto que o Copilot precisa conhecer —, esse contexto deve estar disponível. Os [arquivos de instruções][instruction-files] orientam a geração de código e informam não apenas *o que* você quer, mas também *como* quer que seja feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos de instruções direcionados por caminho.
- gerar a primeira parte da camada de dados para filtragem (um auxiliar de editoras) com as instruções atuais.
- adicionar um novo padrão geral a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado seguir o novo padrão.
- fazer commit da atualização das instruções e da parte de filtragem em `main` para que o Copilot possa usar as novas orientações no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência das mudanças de comportamento após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar tema escuro e ter aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- O início de cada arquivo deve conter um bloco de comentários que descreva sua finalidade.

Os arquivos de instruções garantem que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo e seus requisitos de programação. Elas ajudam a orientar sugestões e trechos de código. Você pode especificar convenções de código, bibliotecas e os tipos de comentários que prefere. As instruções podem se aplicar a todo o repositório ou a tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações, como a pilha tecnológica, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem definir diretrizes para linguagens como TypeScript ou Astro, ou para tarefas como criar componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot [também aceita arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Criar arquivos de instruções em uma conversa completa está além do escopo deste workshop, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo criado, sua estrutura e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou de tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve (ou não deve) ser escrito.

Não existe uma única maneira certa de criar arquivos de instruções ou usar IA. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, há arquivos para várias tarefas, inclusive [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface em uso explicará como fazer isso quando necessário.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório — um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra `.github/instructions` e examine os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um padrão glob relativo à raiz do repositório. Neste caso, arquivos de teste TypeScript, como `**/*.test.ts`, correspondem a ele.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções e arquivos do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos para o Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma branch para as alterações

Vamos seguir as práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e acesse uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando necessário.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente.

1. Feche as guias abertas de exercícios anteriores para que o Copilot receba somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. No seletor de agentes da exibição Chat, selecione **Agent** para que o Copilot possa alterar arquivos.

   ![Captura de tela mostrando o seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, geralmente no arquivo auxiliar e nos testes.
6. Observe que a função sugerida é tipada, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso decorre das convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código sugerido **não tem** comentários TSDoc nem cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a melhora da consistência depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

O arquivo `.github/copilot-instructions.md` contém informações gerais do projeto. Vamos documentar os padrões para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela ainda não exige documentação no código.
3. Logo abaixo dos padrões existentes, adicione estas linhas Markdown:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Os arquivos de instruções podem ficar na raiz do repositório para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é adequado para padrões globais como a regra de comentários recém-adicionada.

## Executar novamente o prompt e observar a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Abra `src/lib/publishers.ts` para que o Copilot se concentre no arquivo.
3. Envie o mesmo prompt de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são controlados pelo mesmo sistema de controle de versão que os demais arquivos. Vamos fazer commit e enviar a branch.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as alterações estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch publicada, crie um pull request associado à issue do backlog sobre as instruções. O Copilot pode encontrar a issue, criar um PR que a encerra e mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub no workspace. Na primeira utilização, o VS Code talvez peça para você entrar no GitHub. Siga as instruções para permitir o acesso. Você aprenderá sobre essa conexão (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace e selecione **Agent**.
2. Use <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat.
3. Peça ao Copilot que encontre a issue das instruções e crie um PR da branch atual, descrevendo também a base do auxiliar de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot procurará a issue e criará o PR. Quando solicitado a permitir ações do GitHub, revise o comando e selecione **Allow** se apropriado.
5. Depois da criação do PR, peça ao Copilot que faça o merge e retorne à branch `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise as solicitações para permitir ações do GitHub e comandos de shell antes de selecionar **Allow**.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *atuais*.
- adicionar um novo padrão geral a `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código adotar o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, você colocará as instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
