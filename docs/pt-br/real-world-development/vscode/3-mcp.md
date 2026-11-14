---
title: "Exercício 3 - Teste seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você criou a filtragem com o modo de agente. Antes de abrir um pull request, confirme se ela funciona no navegador. Conecte o **servidor MCP do Playwright** para que o Copilot teste o recurso em um navegador real. Depois, publique a ramificação e abra o PR.

Neste exercício, você vai:

- aprender como o Model Context Protocol (MCP) amplia o Copilot com ferramentas.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que teste manualmente a filtragem no navegador.
- publicar a ramificação e abrir um pull request.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] é um padrão que permite a agentes de IA acessar ferramentas e serviços externos. Um servidor MCP faz a ponte entre o agente e essas ferramentas, como APIs existentes ou pacotes locais.

![Diagrama que mostra como o modo de agente interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp-server]**: fornece APIs para gerenciar repositórios, problemas e pull requests.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: automatiza um navegador, permitindo navegar em páginas, preencher formulários e selecionar botões.

O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores.

> [!CAUTION]
> Trate servidores MCP como qualquer dependência. Revise o código-fonte, verifique o publicador e considere as implicações de segurança antes de conceder acesso a recursos ou operações sensíveis.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho.

1. No Codespace, abra `.vscode/mcp.json`.
2. Confirme se já existe um servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

O servidor GitHub remoto vem com o modelo do projeto. O VS Code conecta-se a ele por HTTP e solicita autenticação na primeira vez que o Copilot usa uma ferramenta.

## Adicione o servidor MCP do Playwright

Adicione um servidor local do Playwright para que o Copilot controle um navegador.

1. Em `.vscode/mcp.json`, adicione `playwright` junto de `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

O VS Code inicia o servidor local com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária no Codespace.

> [!NOTE]
> O projeto já usa o Playwright para testes de ponta a ponta. Se o Copilot informar que não há navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e autorize o servidor

O VS Code inicia servidores MCP quando necessário e solicita confirmação de confiança.

1. Em `.vscode/mcp.json`, selecione **Start** acima da entrada `playwright` para iniciar o servidor.
2. Se o VS Code solicitar confirmação de confiança, revise a configuração e autorize o servidor.

Iniciar o servidor manualmente é opcional; se você não fizer isso, o VS Code iniciará o servidor quando o Copilot precisar das ferramentas.

## Teste o recurso de filtragem

O MCP do Playwright usa a árvore de acessibilidade da página, não capturas de tela. Assim, o agente examina elementos rotulados, como botões, links e itens de lista.

Com o servidor conectado e o aplicativo em execução, envie este prompt ao Copilot:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Leia o relatório e compare-o com os critérios de aceitação do problema. Se algo estiver incorreto, faça perguntas ou peça a correção antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira utilização, o Playwright talvez precise baixar um navegador; se necessário, execute `npx playwright install chromium`.

## Publique a ramificação e crie um pull request

Depois de confirmar o funcionamento, publique a ramificação `filtering-vscode`.

1. Abra **Source Control** no Codespace e revise as alterações do Copilot.
2. Selecione **+** para preparar as alterações.
3. Use o botão **Sparkle** para gerar uma mensagem de commit.
4. Selecione **Publish** para enviar a ramificação ao repositório.

## Crie o pull request

Peça ao Copilot que encontre o problema relacionado e crie um PR associado a ele.

1. No Copilot Chat, selecione **New Chat** e confirme se **Agent** está selecionado.
2. Envie este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

3. Selecione **Continue** quando necessário para permitir que o Copilot reúna informações e execute ações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar.
4. Abra o link do PR e revise-o, mas **não o mescle ainda**.

## Resumo e próximos passos

Você:

- aprendeu como o Model Context Protocol (MCP) amplia o Copilot com ferramentas.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- pediu ao Copilot que testasse a filtragem no navegador.
- publicou a ramificação e abriu um pull request.

Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
