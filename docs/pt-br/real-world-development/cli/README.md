---
slug: pt-br/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** coloca o GitHub Copilot no terminal como assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem alternar para um editor gráfico.

O workshop acompanha um fluxo de trabalho contínuo no Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma alteração focada para exibir avaliações por estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pelo problema de filtragem, defina a abordagem no modo Plan, implemente-a no modo Autopilot e revise-a no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem no navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise todas as alterações de filtragem e use o Agent Merge no PR do recurso.
9. Explore comandos de barra úteis para contexto, modelos, compartilhamento e delegação opcional à nuvem.

Para manter o foco do workshop, você criará dois PRs: um para as avaliações por estrelas e outro para a filtragem, junto com as atualizações de instruções e da skill, o perfil de QA e os testes. O trabalho de filtragem e qualidade usa a mesma conversa e ramificação para que você possa aproveitar as alterações enquanto explora cada ferramenta. Você também pode seguir uma [série opcional de três módulos com o GitHub Copilot CLI e o Foundry][foundry] para preparar um modelo, criar e implantar um agente hospedado e conectá-lo ao site.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar avaliações por estrelas][ex2] | Alteração de código | Faça uma pequena alteração nos cartões de jogos e abra seu primeiro PR |
| [3. Modos de agente][ex3] | Geração de código | Use os modos Plan e Autopilot para criar um recurso |
| [4. Instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções do projeto |
| [5. Skills de agente][ex5] | Skills | Personalize uma skill reutilizável de verificações |
| [6. Validar com o MCP do Playwright][ex6] | Ferramentas externas | Teste o recurso em um navegador |
| [7. Criar e usar um agente de QA][ex7] | Agentes | Combine os requisitos, as verificações e a validação no navegador |
| [8. Criar e mesclar o PR do recurso][ex8] | Pull requests | Revise as alterações e use o Agent Merge |
| [9. Ferramentas avançadas da CLI][ex9] | Recursos da CLI | Explore contexto, modelos, compartilhamento e delegação opcional |
| [10. Conclusão e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho e conheça os próximos passos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, confirme se o administrador habilitou o Copilot CLI.

## Começar

[**Comece pela Lição 0: Pré-requisitos →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
[callout-student-plan-education]: https://github.com/education/students
