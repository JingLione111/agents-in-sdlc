---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown em `.github/agents` fornecem instruções, práticas recomendadas, padrões de código e conhecimento específico. Também é possível defini-los globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com `name` e `description`, seguido de instruções em Markdown.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] são definidos principalmente em Markdown. Uma forma simples de diferenciá-los: o **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas que o agente pode coordenar. Neste laboratório, o agente de acessibilidade revisará o site conforme as diretrizes e poderá usar skills de fluxo de pull request ou execução de testes.

> [!NOTE]
> Não há uma única maneira certa de escrever um agente. Teste e itere conforme o ambiente e o cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- entender como agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que sua plataforma seja acessível. Pessoas usuárias relataram dificuldade para ler o tema escuro devido ao contraste insuficiente. A equipe solicitou um modo de alto contraste com um controle para ativá-lo ou desativá-lo.

## Revise o agente personalizado de acessibilidade

O projeto já inclui um agente de acessibilidade. Revise suas instruções antes de atribuir a tarefa.

1. No repositório, abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.
3. Examine as responsabilidades, as práticas de acessibilidade, os exemplos de HTML, CSS e JavaScript e as armadilhas comuns.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

## Crie e atribua um problema

O painel de agentes do GitHub é o local central para atribuir e acompanhar tarefas. Crie um problema e atribua-o ao agente de acessibilidade.

1. No repositório, selecione **Issues**, depois **New issue** e **Blank issue**.
2. Defina **Title** como `Add high contrast mode to website`.
3. Defina **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. Selecione **Create** e, à direita, **Assign to Copilot**.
5. Na lista de agentes personalizados, selecione **Accessibility agent** e depois **Assign**.

   ![Atribuição do agente de nuvem com o agente Accessibility selecionado](../../../_images/ex5-select-custom-agent.png)

O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Você:

- entendeu como agentes personalizados são definidos.
- atribuiu uma tarefa a um agente personalizado.

Enquanto o agente implementa o modo de alto contraste, [acompanhe e oriente a sessão][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
