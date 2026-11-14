---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você atribuiu três tarefas ao agente de nuvem do Copilot. Embora ele trabalhe de forma assíncrona e autônoma, é importante acompanhar o progresso. A [página de agentes][agents-page] do GitHub mostra tarefas com pull requests abertos, as operações realizadas e permite orientar sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir o modo de alto contraste, a equipe decidiu adicionar também um modo claro. Oriente a tarefa já em andamento para incluir os dois modos.

## Revise as tarefas do agente de nuvem

Confira o status das tarefas atribuídas ao agente.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas listadas, incluindo documentação, APIs para jogos e o modo de alto contraste.
3. Selecione uma tarefa em andamento e revise as operações, que podem incluir checkout do código, configuração do ambiente, inicialização de servidores MCP e execução da tarefa.
4. Examine o painel do pull request à direita para acompanhar o PR e os arquivos alterados.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

## Oriente o agente de nuvem

Peça ao agente que adicione o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato pode variar.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde até que a configuração seja concluída e o agente comece a trabalhar.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem e caixa de diálogo para orientar a sessão](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Confirme se o Copilot reconhece a solicitação e a incorpora ao trabalho.

O agente incluirá a solicitação depois de concluir a etapa em andamento. Isso pode levar alguns minutos.

## Resumo e próximos passos

Você:

- explorou a página de agentes para acompanhar tarefas do agente de nuvem.
- orientou uma sessão em andamento e solicitou uma funcionalidade adicional.

Em seguida, [itere sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
