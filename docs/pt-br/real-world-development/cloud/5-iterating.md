---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você pediu ao GitHub Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados e implementasse recursos de acessibilidade com modos de alto contraste e claro. Revise os pull requests (PRs) e, se necessário, peça ajustes antes de habilitar a revisão para o restante da equipe.

### Cenário

Os fundamentos de desenvolvimento de software e DevOps não mudam com a IA generativa. Revise o código e siga o processo normal antes de aprovar as alterações do Copilot.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação somente à própria ramificação.
- O agente trabalha em um ambiente efêmero separado, executado no GitHub Actions.
- Uma pessoa precisa aprovar os fluxos de trabalho do GitHub Actions antes da execução.
- O acesso a recursos externos, incluindo servidores MCP, é limitado por padrão.

## Revise a documentação gerada

Comece pelo PR que adiciona documentação ao código.

> [!NOTE]
> Um aviso pode indicar que o Copilot foi bloqueado por um firewall. Isso é esperado, pois o acesso a recursos externos é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. No github.com, abra o repositório e selecione **Pull Requests**.
2. Abra o PR com um título semelhante a **Add missing documentation**. Se ele ainda tiver o rótulo **[WIP]**, aguarde a conclusão do trabalho.
3. Selecione **Files changed** e revise as alterações. Procure comentários TSDoc, convenções TypeScript e cabeçalhos, de acordo com as instruções revisadas no Exercício 2.

   ![Guia Files changed de um pull request](../../../_images/shared-pr-files-changed.png)
4. Volte à guia **Conversation**. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)
5. Na seção de verificações do PR, confirme se as verificações do aplicativo Astro foram aprovadas.
6. Adicione um comentário marcando **@copilot** para pedir uma melhoria, por exemplo, cabeçalhos nos arquivos, comentários TSDoc ou um README que descreva a estrutura do aplicativo.
7. Selecione **View Session** para acompanhar a nova sessão; use **Back to pull request** para voltar ao PR.

   ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)
8. Quando o Copilot concluir, revise o novo commit e as alterações.

Itere até que o resultado esteja correto. Depois, você poderá marcar o PR como pronto para revisão e mesclá-lo.

![Opção para converter o pull request em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Revise o PR que mostra jogos relacionados na página de detalhes.

1. No github.com, abra **Pull Requests** e selecione o PR com título semelhante a **Show related games on the game details page**.
2. Em **Files changed**, revise o código.
3. Volte à guia **Conversation** e, se necessário, selecione **Approve and run workflows**. Confirme se as verificações do aplicativo Astro foram aprovadas.
4. Opcionalmente, teste manualmente a ramificação no Codespace. Substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Você pode pedir ajustes com um comentário na guia **Conversation**, como alterar a quantidade de jogos exibidos ou adicionar comentários TSDoc e cabeçalhos.

## Revise os recursos de acessibilidade

Por fim, revise o PR do agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no Exercício 4.

1. No github.com, abra **Pull Requests** e selecione o PR com título semelhante a **Add high contrast mode to website**. Se ele ainda tiver **[WIP]**, aguarde a conclusão.
2. Em **Files changed**, revise os controles dos modos, a persistência com armazenamento local, os estilos e os recursos de acessibilidade, como rótulos ARIA e navegação por teclado.
3. Volte à guia **Conversation** e, se necessário, selecione **Approve and run workflows**. Confirme se as verificações foram aprovadas.
4. Opcionalmente, teste a ramificação no Codespace:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

5. Inicie o aplicativo e teste os modos de alto contraste e claro no navegador, incluindo a persistência após recarregar a página.
6. Se identificar problemas de acessibilidade, comente no PR e marque **@copilot** para solicitar alterações.

## Exercício opcional: continue delegando

O agente de nuvem é útil para itens reais do backlog. Crie mais problemas e atribua-os ao Copilot, por exemplo, um formulário de interesse de apoiadores, paginação na lista de jogos ou validação de entrada nos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Você:

- inspecionou as instruções personalizadas do repositório.
- atribuiu problemas ao agente de nuvem e acompanhou a configuração e a execução assíncronas.
- criou e usou um agente personalizado de acessibilidade para adicionar modos de alto contraste e claro.
- usou a página de agentes para monitorar e orientar a sessão.
- revisou e iterou sobre pull requests do agente de nuvem, marcou **@copilot** para solicitar alterações e aprovou fluxos de trabalho.

## Revisão e próximos passos

Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- **[Percurso do VS Code](../../vscode/)**: explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- **[Percurso da CLI](../../cli/)**: entregue duas alterações revisadas pelo terminal com Plan, Autopilot, skills, um agente de QA, MCP do Playwright, Agent Merge e delegação opcional ao agente de nuvem.

No seu repositório, experimente atribuir ao agente de nuvem um problema de refatoração ou cobertura de testes, criar um agente personalizado para outro domínio ou configurar `copilot-setup-steps.yml` para outra pilha.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir problemas do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

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
