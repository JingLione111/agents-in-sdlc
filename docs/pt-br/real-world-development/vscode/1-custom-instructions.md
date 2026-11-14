---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] fornecem ao Copilot informações sobre o que fazer e como fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos com escopo de caminho.
- gerar a primeira parte dos dados de filtragem, um helper de editoras, com as instruções atuais.
- adicionar um padrão geral do repositório a `.github/copilot-instructions.md`.
- repetir o mesmo prompt e observar o código gerado aplicar o novo padrão.
- confirmar a atualização das instruções e a parte do recurso em `main` para o próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões. O Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter exatamente o mesmo texto em cada execução.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes unitários; a interface deve usar modo escuro e ter aparência moderna; o código deve incluir comentários TSDoc; e cada arquivo deve começar com um bloco de comentários que descreva sua finalidade. Os arquivos de instruções ajudam o Copilot a seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot. Elas podem especificar convenções de código, bibliotecas e comentários preferidos. Você pode criar instruções para todo o repositório ou para arquivos e tarefas específicos.

- `.github/copilot-instructions.md` é enviado ao Copilot em todas as solicitações do repositório e deve conter contexto e orientações gerais do projeto.
- `.github/instructions/*.instructions.md` fornece diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código ou sugestões de próxima edição. O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`. O Copilot também oferece suporte a outros padrões, como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado nas informações do projeto e nos padrões gerais.
- Use `*.instructions.md` para instruções específicas de tipos de arquivo ou tarefas.
- Escreva em linguagem natural, de forma clara, e inclua exemplos do que fazer e do que evitar.

Não há uma única maneira certa de criar arquivos de instruções; experimente e descubra o que funciona para o projeto.

> [!TIP]
> O projeto inclui instruções para tarefas como [alterações na interface][ui-instructions] e [arquivos Astro][astro-instructions]. O Copilot também pode ajudar a gerar instruções. Para modelos, explore o [Awesome Copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

Leia o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` incluídos no repositório.

1. Abra `.github/copilot-instructions.md` e observe a descrição do projeto e as seções **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação **GitHub Actions Workflows**.
2. Explore `.github/instructions`, que contém instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob relativo à raiz do repositório. Por exemplo, ele corresponde a arquivos de teste TypeScript como `**/*.test.ts`.
4. Leia as orientações específicas para criar testes unitários.
5. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e arquivos do projeto. Esses caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** descreve os padrões de código, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Crie uma ramificação para as alterações

Mantenha este trabalho em uma ramificação separada.

1. Volte ao Codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma ramificação:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

Comece com um prompt usando as instruções atuais e, depois, repita-o para comparar o resultado.

1. No Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa.
2. Feche as guias do editor de exercícios anteriores e abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na lista de agentes da visualização Chat, selecione **Agent**.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)
4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Aguarde enquanto o Copilot explora o projeto e aplica as alterações, que podem incluir o helper e os testes.
6. Observe que o helper proposto é tipado, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe se o código proposto está sem comentários TSDoc e sem cabeçalho de arquivo.

> [!CAUTION]
> O Copilot pode adicionar comentários mesmo sem a instrução. Isso não é um problema; o objetivo é observar a consistência após a atualização.

## Adicione um padrão ao repositório

O arquivo `.github/copilot-instructions.md` contém orientações gerais do projeto. Acrescente os padrões de documentação do código.

1. Reabra `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, perto da linha 27, e observe que ainda não há regras de documentação.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> O arquivo do repositório é o lugar certo para padrões gerais. Use arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas.

## Repita o prompt e observe a alteração

Use o mesmo prompt para ver como as novas instruções afetam o código proposto.

1. No Copilot Chat, selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` e envie novamente o prompt anterior.
3. Observe se o arquivo proposto começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

4. Confirme se a função inclui um comentário TSDoc que descreva a finalidade, o parâmetro `db` e o valor retornado.

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código usado no próximo exercício.

## Confirme as instruções e envie a ramificação

Os arquivos de instruções são gerenciados pelo controle do código-fonte, como os demais arquivos do repositório.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Execute `git status` e confirme se as instruções e o helper estão presentes.
3. Prepare e confirme as alterações:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a ramificação:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Crie um pull request associado ao problema de documentação de padrões no backlog.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) e confirme se **Agent** está selecionado.
3. Envie este prompt:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando solicitado, revise a ação do GitHub e selecione **Allow** se apropriado.
5. Depois que o PR for criado, peça ao Copilot que o mescle em `main` e atualize a cópia local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise as solicitações para executar ações do GitHub ou comandos do shell e selecione **Allow** quando apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa os arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um padrão geral do repositório a `.github/copilot-instructions.md`.
- repetir o prompt e observar o código proposto seguir o novo padrão.
- confirmar as instruções e o helper em `main` para o próximo exercício.

Em seguida, use essas instruções no [modo de agente][next-lesson] para adicionar um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
