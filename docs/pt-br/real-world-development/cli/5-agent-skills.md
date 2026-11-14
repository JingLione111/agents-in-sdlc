---
title: "Lição 5 - Personalize e use uma skill de quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Já validamos manualmente o funcionamento do código e usamos arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, as **skills de agente** são ideais. As skills ajudam o Copilot a entender como executar operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para executar testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis, scripts executáveis e recursos complementares que o agente carrega quando necessário. No mínimo, uma skill é uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O arquivo inclui frontmatter com nome e descrição, uma visão geral da função da skill e orientações sobre quando usá-la. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` do Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta de skills do usuário, em `~/.copilot/skills`.

## Explore a skill

Explore a skill `quality-checks` criada pela equipe Tailspin Toys para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando deve usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot no processo de testes e lint.
4. Observe que a skill ainda não inclui uma seção **Formato de saída dos resultados**.

## Execute a skill antes de fazer alterações

As skills podem ser chamadas diretamente no Copilot CLI ou por meio de linguagem natural. Peça ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado ao final.

## Personalize o relatório

Melhore o relatório para informar o que foi executado, se houve sucesso e o que as ferramentas relataram. Atualize a skill para produzir esse formato.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione esta seção ao final do arquivo:

   ```markdown
   ## Formato de saída dos resultados

   Ao concluir, informe cada comando executado e se ele foi aprovado, falhou ou ficou bloqueado. Inclua contagens de testes, durações, erros, avisos e outras métricas somente quando forem informados pela ferramenta. Indique a próxima ação para cada falha ou bloqueio e nunca descreva uma verificação ignorada ou incompleta como aprovada.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa. Execute novamente a skill e compare os relatórios.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente. Você:

- explorou a skill `quality-checks` para executar testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, permita que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
