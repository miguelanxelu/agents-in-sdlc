---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo podem exigir alterações em vários arquivos e tarefas como executar testes. Em geral, os desenvolvedores precisam localizar os arquivos relevantes, fazer alterações, executar testes, depurar, identificar o que ficou faltando e repetir o processo. A lista continua.

O modo de agente do Copilot ajuda a coordenar esse trabalho. No IDE, ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Veja como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo de agente do Copilot explora o projeto, identifica os arquivos relevantes e coordena as alterações.
- o modo de agente implementa funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, permita que as pessoas filtrem por categoria e editora. Você já criou um helper de editoras no exercício anterior; agora conclua com o modo de agente o restante do trabalho na camada de dados, na interface e nos testes.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site para conhecer a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele lista jogos e exibe seus detalhes. O site é um único aplicativo Astro que renderiza páginas HTML estáticas durante a compilação. As páginas consultam um banco de dados SQLite local diretamente no frontmatter, usando o mapeador objeto-relacional (ORM) Drizzle. Não há uma API (interface de programação de aplicações) de back-end separada nem um framework de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com scripts Astro `<script>` pequenos e específicos, usando APIs do modelo de objetos do documento (DOM).

### Inicie o site

O comando `npm run dev` inicia o único servidor de desenvolvimento Astro. Execute-o no GitHub Codespaces:

1. Volte ao Codespace. Depois do exercício anterior, você deverá estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um banner com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Mantenha <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecione o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link localhost no terminal redireciona para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço é um túnel privado para o servidor em execução no Codespace.

### Explore o site

Com o site em execução, explore suas funcionalidades:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: selecionar um jogo abre uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indica o que é necessário.

O site funciona, mas você quer adicionar recursos. Comece examinando o backlog de issues que foi criado automaticamente a partir do modelo do projeto. Peça ao GitHub Copilot que mostre os itens.

1. Na lista de agentes da visualização Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e usa ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores do workshop não estão recomendando um modelo específico. O Claude Sonnet 4.5 foi usado para criar este workshop, com o objetivo de obter sugestões relativamente consistentes. Como os modelos de linguagem grandes (LLMs) são probabilísticos, suas sugestões podem ser diferentes dos exemplos. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando do Model Context Protocol (MCP) diferente, mas ainda deverá concluir a tarefa.

3. Envie ao Copilot este prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude a priorizá-los com base no que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens conforme a utilidade que atribuiu a cada um.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, examine o arquivo de instruções que fornecerá contexto ao Copilot. O arquivo [de instruções de interface][ui-instructions] contém orientações para adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, incluindo Astro, Tailwind CSS (folhas de estilo em cascata) e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar instruções, dividir conjuntos complexos em partes menores e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, faça pelo menos três alterações:

- adicione ou refine a lógica de filtragem na camada de dados, em `src/lib/`.
- adicione ou atualize testes do comportamento de filtragem.
- atualize a página de listagem de jogos para incluir a interface de filtragem.

Execute e aprove os testes antes de mesclar as alterações. O modo de agente do Copilot pode realizar essas tarefas.

1. Continue a conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Selecionar o arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado na lista de agentes da visualização Chat.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Use este prompt para pedir ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Atualize o site para permitir a filtragem por editora e categoria, de acordo com os requisitos da issue relacionada no backlog do GitHub. Já existe um helper de editoras do exercício anterior; preserve e refine esse trabalho conforme necessário ao concluir o restante da camada de dados, da interface e dos testes. Confirme que todos os testes passam. O servidor já está em execução, portanto não é necessário iniciá-lo.
   ```

6. Acompanhe o Copilot explorando o projeto e localizando os helpers da camada de dados, a interface e os testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot explorará o projeto, alterará arquivos e executará testes. Isso pode levar alguns minutos, dependendo da tarefa e da base de código. Durante o processo, os controles **Keep** e **Undo** podem aparecer no editor. Aguarde a conclusão antes de aceitar ou desfazer as alterações.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Solicitação para confirmar a execução dos testes no painel Copilot Chat](../../../_images/ex3-agent-mode-run-tests.png)

8. Alguns testes podem falhar durante o processo. O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de inteligência artificial.

10. Volte ao navegador em que o site está em execução e experimente a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site da Tailspin Toys. Nesta lição, você:

- usou o modo de agente para implementar funcionalidades na interface e na camada de dados.
- explorou o projeto, identificou arquivos relevantes e coordenou alterações.
- revisou as alterações e os testes gerados antes de mesclá-los à base de código.

Em seguida, [teste a funcionalidade com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implemente paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use o que aprendeu para pedir ao Copilot que atualize o site. Considere:

- seguir as práticas existentes e usar os arquivos de instruções.
- decidir se as pessoas poderão selecionar o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Essa interação é normal ao trabalhar com o Copilot.

## Recursos

- [Copilot: modos Ask, Edit e Agent — o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
