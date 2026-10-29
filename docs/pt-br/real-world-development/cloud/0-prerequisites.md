---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do agente de nuvem do Copilot, prepare tudo. Crie sua cópia do repositório Tailspin Toys e inicie um [codespace][codespaces] para editar arquivos de instruções e revisar o trabalho produzido pelo agente de nuvem.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório onde você fará as alterações, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você participa de um evento organizado pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do modelo do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que criou (**nome-da-organização-ou-usuário/nome-do-repositório**); você o usará mais adiante.

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

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório. Você também iniciou a criação do codespace, que usará enquanto trabalha com o agente de nuvem do Copilot.

## Próxima etapa

Vamos adicionar instruções personalizadas que o agente de nuvem seguirá. Continue para [Exercício 1: Instruções personalizadas][next-lesson].

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório com base em um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
