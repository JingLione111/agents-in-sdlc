---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do agente de nuvem do Copilot, prepare o ambiente. Você criará uma cópia do repositório Tailspin Toys e iniciará um [Codespace][codespaces] para editar arquivos de instruções e revisar o trabalho do agente.

## Configure o repositório do laboratório

Crie uma cópia do repositório a partir do [modelo][template-repository]. A cópia conterá os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e, em seguida, **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório (**organization-or-user-name/repository-name**) para usá-lo mais tarde.

   ![Campos de configuração do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

## Crie um Codespace

Use um Codespace para concluir os exercícios. O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código no navegador, com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que você criou.
2. Selecione o botão verde **Code**.

   ![Botão Code na página do repositório](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um Codespace.

   ![Opção para criar um novo Codespace](../../../_images/ex0-create-codespace.png)

A criação pode levar alguns minutos, mas ainda é mais rápida do que instalar todos os serviços manualmente.

> [!CAUTION]
> Você voltará ao Codespace em outro exercício. Por enquanto, mantenha-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop pode ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado.

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação de um Codespace para usar em conjunto com o agente de nuvem do Copilot.

## Próxima etapa

Adicione as instruções personalizadas que o agente de nuvem seguirá no [Exercício 1: Instruções personalizadas][next-lesson].

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[next-lesson]: ../1-custom-instructions/
