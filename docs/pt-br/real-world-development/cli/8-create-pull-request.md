---
title: "Lição 8 - Crie e mescle o PR do recurso"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única ramificação. Agora, revise tudo em conjunto e abra um pull request. Você mesmo mesclou o pull request (PR) de avaliação por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs. Para agilizar o workshop, mantivemos todo o trabalho de filtragem e qualidade em uma conversa e ramificação, e incluiremos tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a alteração do recurso e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar a mesclagem do PR.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização para mesclar sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para concluir um pull request. Quando você o habilita, o Copilot resolve o que estiver bloqueando o PR: corrige falhas nas verificações de integração contínua (CI), responde a comentários de revisão e faz rebase quando necessário. Em seguida, habilita a mesclagem automática do GitHub, se o repositório permitir.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme se ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado, revise as alterações, crie o PR e deixe o Agent Merge gerenciar as próximas etapas.

1. Volte ao Codespace.
2. Digite `/agent` para abrir a caixa de diálogo do agente.
3. Na lista, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR e fará a mesclagem quando tudo for concluído com êxito.
7. Confirme se o Copilot mostra uma mensagem semelhante a "PR #14 was squash-merged successfully".

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de ramificação, filas de mesclagem, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento: geração e validação de código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou toda a alteração do recurso e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou a mesclagem do PR.

Em seguida, você explorará outros comandos de barra do Copilot CLI para gerenciar contexto, modelos e compartilhamento e, opcionalmente, delegar tarefas à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
