---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` definem instruções, práticas recomendadas, padrões de código e conhecimento específico. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade, segurança ou testes.

Os agentes podem ser definidos em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] usam arquivos Markdown para orientar a IA. Uma maneira simples de diferenciá-los: o **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas. Agentes têm sua própria janela de contexto e podem coordenar skills ou outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes e poderá usar skills de fluxo de pull request ou execução de testes.

> [!NOTE]
> Não há uma única maneira certa de escrever um agente personalizado. Teste e itere para encontrar o que funciona no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente no Copilot Chat para implementar um modo de alto contraste.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todos. Pessoas usuárias relataram que o tema escuro é difícil de ler devido ao contraste insuficiente. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

## Revise o agente personalizado de acessibilidade

O projeto já inclui um agente de acessibilidade. Examine suas instruções antes de usá-lo.

1. No Codespace, volte para `main`, atualize-a e crie uma ramificação separada para acessibilidade:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md` e observe os campos `name` e `description` no frontmatter YAML.
3. Examine as responsabilidades, as práticas de acessibilidade, os exemplos de HTML, CSS e JavaScript e as armadilhas comuns.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

## Use o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados de `.github/agents` na lista de agentes do Copilot Chat. Inicie uma conversa limpa e selecione o agente de acessibilidade.

1. No Copilot Chat, selecione **New Chat** e confirme se **Agent** está selecionado.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de agentes, selecione **Accessibility agent**.
3. Envie este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. Acompanhe as alterações propostas no chat e o indicador **Files changed**.

O agente pode levar alguns minutos para atualizar componentes Astro, CSS e testes relacionados. Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Você:

- revisou um agente personalizado de acessibilidade existente.
- usou o agente no Copilot Chat para implementar um modo de alto contraste.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações e peça um controle de modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no Awesome Copilot][awesome-copilot-agents]

| [← Lição anterior: Teste seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
