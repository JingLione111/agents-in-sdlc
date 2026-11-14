---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Muitas organizações enfrentam dívida técnica, como problemas de segurança pendentes, código legado ou solicitações que ficaram no backlog. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir, ele abre um pull request de rascunho para revisão humana.

Neste exercício, você vai:

- personalizar o ambiente de geração de código.
- entender como as operações são protegidas.
- reconhecer a importância de problemas bem delimitados.
- atribuir problemas ao Copilot.

## Cenários

A Tailspin Toys quer adicionar comentários TSDoc às funções exportadas. A equipe de design também quer mostrar jogos relacionados na página de detalhes — outros títulos da mesma categoria — para testar a experiência. São tarefas que podem ser delegadas ao agente de nuvem enquanto você trabalha em outras prioridades.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] trabalha em segundo plano, configura um ambiente, analisa o repositório e executa a tarefa atribuída. Você pode atribuir um problema do GitHub ao Copilot, acompanhar a sessão e revisar o pull request quando o trabalho estiver pronto.

## A importância de instruções bem delimitadas

O GitHub Copilot não elimina a necessidade de pensar. Divida as tarefas em etapas e defina escopo claro, conforme as [práticas recomendadas do agente de nuvem][cloud-agent-best-practices].

## Instruções personalizadas neste repositório

Antes de atribuir trabalho, examine as instruções existentes, que orientarão os resultados.

1. Abra `.github/copilot-instructions.md` no github.com ou no Codespace. Revise **Code standards**, especialmente as convenções TypeScript e os comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir o problema `Code lacks documentation`, observe nos resultados os comentários TSDoc, as convenções TypeScript e os cabeçalhos de arquivo descritos nas instruções.

## Configure o ambiente de desenvolvimento do agente

O agente de nuvem usa o GitHub Actions para configurar seu ambiente. O fluxo especial `.github/workflows/copilot-setup-steps.yml` é executado antes do trabalho e pode instalar ferramentas e dependências. O projeto já inclui esta configuração:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

O fluxo contém um job `copilot-setup-steps`, executado antes do agente, e o gatilho `workflow_dispatch`, que permite testá-lo manualmente na guia **Actions**.

## Adicione documentação ao código

Crie um problema para que o agente documente as funções exportadas.

1. No github.com, abra o repositório e selecione **Issues**.
2. Selecione **New issue** e **Blank issue**.
3. Defina **Title** como `Code lacks documentation`.
4. Defina **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

5. Selecione **Create** e, à direita, **Assign to Copilot**.

   ![Atribuição do Copilot a um problema](../../../_images/shared-assign-copilot.png)

6. Revise os detalhes da atribuição e selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

7. Abra **Pull Requests** e selecione o pull request (PR) recém-criado, com título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde e atualize a página.
8. Aguarde a lista de tarefas do Copilot e revise-a; a criação do ambiente e a análise podem levar alguns minutos.
9. Na linha do tempo do PR, selecione **View session** para acompanhar a sessão.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

O agente trabalha de forma assíncrona. Enquanto isso, atribua outra tarefa.

## Adicione jogos relacionados à página de detalhes

Delegue ao agente a criação de uma seção de jogos relacionados.

1. No github.com, abra **Issues**, selecione **New issue** e **Blank issue**.
2. Defina **Title** como `Show related games on the game details page`.
3. Defina **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

4. Selecione **Create**, depois **Assign to Copilot** e **Assign**.

   ![Atribuição do Copilot a um problema](../../../_images/shared-assign-copilot.png)

5. Confirme se o problema mostra o indicador de que o Copilot está trabalhando.

   ![Emoji de olhos indicando que o Copilot está trabalhando no problema](../../../_images/ex4-issue-eyes-emoji.png)

## Resumo e próximos passos

Você:

- personalizou o ambiente de geração de código.
- entendeu como as operações são protegidas.
- reconheceu a importância de problemas bem delimitados.
- atribuiu problemas ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O agente de nuvem também oferece suporte a servidores MCP e instruções personalizadas.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir problemas do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
