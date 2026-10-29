---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo. Crie sua cópia do repositório Tailspin Toys, inicie um [codespace][codespaces] para trabalhar e confirme que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório onde você fará as alterações, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você participa de um evento organizado pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do modelo do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que criou (**nome-da-organização-ou-usuário/nome-do-repositório**); você o usará mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório com base no modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop; não precisa criar nenhuma.

## Criar um codespace

Agora, use um codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um codespace.

    ![Criar um codespace](../../../_images/ex0-create-codespace.png)

A criação pode levar alguns minutos, embora ainda seja muito mais rápida do que instalar todos os serviços manualmente. Enquanto isso, explore outros recursos do GitHub Copilot, que veremos a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos oferecem os pré-requisitos necessários. Para executá-lo localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado; o VS Code criará o mesmo contêiner de desenvolvimento usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension]. Você também pode abrir a exibição Extensões no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar-se com sua conta do GitHub.

1. Volte ao codespace.
2. Se o Copilot Chat não estiver visível à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione Enter. Isso deve ativá-lo.
4. Se você ainda não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para autenticar-se.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório. Você também iniciou a criação do codespace, que usará para escrever código.

## Próxima etapa

Vamos começar a usar o Copilot. Continue para [Exercício 1: Instruções personalizadas][next-lesson], onde você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório com base em um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
