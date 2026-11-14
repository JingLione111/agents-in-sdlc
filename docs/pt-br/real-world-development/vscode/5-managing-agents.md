---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot explora o código, propõe alterações, edita arquivos e executa comandos. No VS Code, você acompanha as chamadas de ferramentas e as edições, revisa diffs e orienta a conversa sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas.
- orientar o agente na mesma sessão para adicionar um controle de modo claro.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. A equipe de design também solicitou um modo claro. Oriente a sessão existente para ampliar o trabalho sem perder o contexto.

## Acompanhe o progresso do Copilot

O Copilot Chat mostra os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece com um diff e controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Acompanhe as chamadas de ferramentas, leituras e edições enquanto o agente trabalha.
3. Para cada diff, selecione **Keep** para aceitar a alteração ou **Undo** para revertê-la.
4. Abra **Files changed** para revisar o conjunto de arquivos alterados.
5. Aguarde a conclusão do modo de alto contraste e leia o resumo final.

> [!NOTE]
> Se uma chamada pedir permissão, por exemplo, para executar um comando do terminal, revise o comando e aprove somente se estiver de acordo.

## Oriente o agente para adicionar o modo claro

Continue a mesma conversa para que o agente mantenha o contexto dos componentes, dos estilos e do padrão de armazenamento local.

1. Envie este prompt complementar no Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Revise cada nova edição e selecione **Keep** ou **Undo**.
3. Se o agente seguir uma direção indesejada, selecione **Stop** e envie uma orientação mais clara.

> [!TIP]
> Prompts complementares permitem ajustar, corrigir o rumo ou ampliar o escopo sem reiniciar a conversa.

## Revise o conjunto de trabalho

Antes de confirmar as alterações, revise tudo que o agente modificou.

1. Abra **Source Control** no VS Code e revise a lista de arquivos alterados.
2. Examine alguns diffs e confirme se há atributos ARIA, navegação por teclado, HTML semântico e persistência com armazenamento local.
3. Quando estiver satisfeito, prepare e confirme as alterações no painel Source Control. Você as publicará na próxima lição.

## Resumo e próximos passos

Você:

- acompanhou em tempo real o trabalho do agente de acessibilidade.
- revisou e aceitou as edições propostas.
- orientou o agente na mesma sessão para adicionar um controle de modo claro.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
