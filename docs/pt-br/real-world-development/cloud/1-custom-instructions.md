---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] fornecem orientação para que o Copilot entenda não apenas o que fazer, mas também como.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos com escopo de caminho.
- adicionar um padrão geral do repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não fará uma comparação antes/depois. O agente de nuvem trabalha de forma assíncrona com problemas do GitHub; você observará o efeito das instruções ao revisar os pull requests que ele criar.

## Arquivos de instruções

### Cenário

A Tailspin Toys exige testes unitários para a camada de dados, modo escuro e aparência moderna na interface, comentários TSDoc no código e um bloco de comentários no início de cada arquivo que explique sua finalidade. Os arquivos de instruções ajudam o Copilot a seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot. Você pode especificar convenções, bibliotecas e comentários preferidos para todo o repositório ou para arquivos e tarefas específicos.

- `.github/copilot-instructions.md` é enviado ao Copilot em todas as solicitações do repositório e deve conter contexto e orientações gerais.
- `.github/instructions/*.instructions.md` fornece diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código ou sugestões de próxima edição. O Copilot Chat, o Copilot CLI e o agente de nuvem usam as instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`. O Copilot também oferece suporte a outros padrões, como AGENTS.md e CLAUDE.md.

Boas práticas: mantenha as instruções gerais em `copilot-instructions.md`; use arquivos `*.instructions.md` para tipos de arquivo ou tarefas específicas; escreva orientações claras em linguagem natural e inclua exemplos. Não há uma única maneira certa de criar instruções. O projeto também inclui arquivos para [alterações na interface][ui-instructions] e [arquivos Astro][astro-instructions]. Para modelos, explore o [Awesome Copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Leia os arquivos incluídos no repositório para observar quais orientações o agente poderá usar.

1. Abra `.github/copilot-instructions.md` e examine **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação **GitHub Actions Workflows**.
2. Explore `.github/instructions`, que contém instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe `applyTo`, que define um glob relativo à raiz do repositório, como `**/*.test.ts`.
4. Leia as orientações específicas para criar testes unitários.
5. Abra `.github/instructions/drizzle.instructions.md` e examine os links para instruções e arquivos do projeto, relativos ao próprio arquivo.

> [!NOTE]
> A seção **Code formatting requirements** ainda não exige documentação no código. Você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicione um padrão ao repositório

O agente de nuvem usará as instruções da ramificação de destino do problema. Adicione e mescle o padrão em `main` para que ele esteja disponível no próximo exercício.

1. No terminal do Codespace, crie e selecione uma ramificação:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra `.github/copilot-instructions.md` e localize **Code formatting requirements**, perto da linha 27.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.
5. Confirme e envie a alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   git push -u origin custom-instructions-cloud
   ```

6. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e mescle-o.

> [!TIP]
> Você também pode trabalhar diretamente em `main` e ignorar a etapa de ramificação. A ramificação é usada para refletir o fluxo de trabalho de um projeto real.

## Resumo e próximos passos

Você:

- explorou como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos com escopo de caminho.
- adicionou um padrão geral do repositório a `.github/copilot-instructions.md`.

Você verá o efeito desse padrão nos pull requests do agente de nuvem.

Em seguida, [atribua seu primeiro problema ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
