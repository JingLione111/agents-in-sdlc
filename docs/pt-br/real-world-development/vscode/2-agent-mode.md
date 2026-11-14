---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma pequena atualização de aplicativo pode exigir alterações em vários arquivos, execução de testes e depuração. O modo de agente do Copilot pode explorar a estrutura do projeto, atualizar arquivos, executar tarefas e corrigir problemas encontrados. Neste exercício, use-o para adicionar uma funcionalidade ao site.

> [!NOTE]
> O modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE e permite ciclos de feedback rápidos. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando problemas e pull requests.

Neste exercício, você vai:

- explorar o projeto e identificar arquivos relevantes com o modo de agente.
- implementar uma funcionalidade na interface e na camada de dados.
- revisar as alterações e os testes antes da mesclagem.

## Cenário

À medida que a lista de jogos cresce, os usuários precisam filtrar por categoria e editora. Você já criou um helper de editoras na lição anterior; agora concluirá o trabalho na camada de dados, na interface e nos testes.

## Execute o site Tailspin Toys

Antes de alterar o código, explore o site para entender sua funcionalidade atual. O Tailspin Toys é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. O aplicativo Astro gera HTML estático; as páginas consultam diretamente um banco de dados SQLite local com Drizzle ORM. Não há API de back-end separada nem framework de interface no cliente. Os helpers de acesso a dados ficam em `src/lib/`, e a interatividade usa scripts Astro pequenos com APIs padrão do DOM.

### Inicie o site

Inicie o servidor de desenvolvimento e crie uma ramificação para o trabalho de filtragem.

1. Volte ao Codespace e confirme se você está em `main`.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Atualize `main` e crie a ramificação:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

5. Quando o servidor Astro estiver pronto, abra `http://localhost:4321` mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço no terminal.

No Codespace, o link local redireciona para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor.

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura do navegador para `http://localhost:4321`, feche-a selecionando **x**.

### Explore o site

Na página inicial, a lista apresenta jogos de tabuleiro, títulos, imagens e descrições. Ao selecionar um jogo, a página de detalhes mostra mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

Peça ao Copilot que mostre os problemas criados automaticamente a partir do modelo.

1. No Codespace, abra o Copilot Chat e selecione **New Chat**.
2. Na lista de agentes da visualização Chat, selecione **Agent**.
3. Escolha **Claude Sonnet 4.5** na lista de modelos.
4. Envie este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

5. Selecione **Continue** para permitir que o comando liste os problemas e revise a lista gerada.

> [!CAUTION]
> O workshop não recomenda um modelo específico. O modelo citado foi usado ao criar o conteúdo, mas as respostas podem variar por serem probabilísticas.

> [!NOTE]
> Por causa da natureza probabilística dos modelos de linguagem grandes (LLMs), o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

## Revise os arquivos de instruções

Antes de solicitar a implementação, leia as instruções de interface do usuário (UI) que orientam o trabalho.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Examine a arquitetura, os princípios de design de componentes, testabilidade e acessibilidade, e os links para instruções de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os caminhos nos arquivos de instruções são relativos ao próprio arquivo. Assim, é possível dividir orientações complexas e incluir exemplos reutilizáveis.

## Implemente a filtragem

A implementação envolve a lógica restante na camada de dados (`src/lib/`), os testes de filtragem e a interface na página de listagem de jogos. Peça ao modo de agente que coordene essas alterações.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui**.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** e **Claude Sonnet 4.5** estão selecionados.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

5. Acompanhe o Copilot enquanto ele explora os helpers de dados, a interface e os testes. Se solicitado, selecione **Continue** para executar os testes.

   ![Solicitação no painel Chat para confirmar a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

6. Revise o código e os testes propostos. Durante o trabalho, não é necessário selecionar **Keep** ou **Undo**; aguarde as alterações finais.

   ![Sessão concluída do modo de agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)
7. Volte ao navegador, experimente os filtros e, depois da revisão, selecione **Keep** na janela do Copilot Chat.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

## Resumo e próximos passos

Você:

- explorou o projeto e identificou arquivos relevantes com o modo de agente.
- implementou uma funcionalidade na interface e na camada de dados.
- revisou as alterações e os testes antes da mesclagem.

Em seguida, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício bônus: implemente paginação

À medida que a lista de jogos cresce, a paginação pode ser necessária. Peça ao Copilot que a implemente usando os arquivos de instruções existentes. Decida se o usuário poderá escolher o tamanho da página, forneça as orientações necessárias e itere sobre a implementação.

## Recursos

- [Modos Ask, Edit e Agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
