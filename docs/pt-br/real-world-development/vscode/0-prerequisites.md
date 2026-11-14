---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare o ambiente. Você criará uma cópia do repositório Tailspin Toys, iniciará um [Codespace][codespaces] e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configure o repositório do laboratório

Crie uma cópia do repositório a partir do [modelo][template-repository]. A cópia conterá os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e, em seguida, **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele durante o laboratório.

   ![Campos de configuração do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

## Crie um Codespace

Use um Codespace para concluir os exercícios. O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código no navegador. Ele oferece um IDE completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que você criou.
2. Selecione o botão verde **Code**.

   ![Botão Code na página do repositório](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um Codespace.

   ![Opção para criar um novo Codespace](../../../_images/ex0-create-codespace.png)

A criação pode levar alguns minutos, mas ainda é mais rápida do que instalar todos os serviços manualmente.

> [!CAUTION]
> Você voltará ao Codespace em outro exercício. Por enquanto, mantenha-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop pode ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Use o GitHub Copilot Chat e o modo de agente

Para usar o modo de agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar instalada se você estiver usando um Codespace.

> [!TIP]
> Se a extensão não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension] ou abra a visualização Extensions, pesquise **GitHub Copilot Chat** e selecione **Install**.

Depois da instalação, talvez seja necessário autenticar sua conta do GitHub.

1. Volte ao Codespace.
2. Se o Copilot Chat não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione <kbd>Enter</kbd>.
4. Se solicitado, siga as instruções para entrar na sua conta do GitHub.

   ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)
5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação de um Codespace para usar ao começar a programar.

## Próxima etapa

Continue com [Exercício 1: Instruções personalizadas][next-lesson] para apresentar ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
