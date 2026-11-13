---
title: "Exercício 3 - Testar a funcionalidade com o servidor do Model Context Protocol (MCP) do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de implementar a filtragem com o modo de agente. Antes de abrir um pull request (PR), confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o servidor do Model Context Protocol (MCP) do Playwright e deixe o Copilot controlar um navegador real. Depois, publique a branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o MCP e como servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais poderoso quando pode acessar recursos além do editor. O MCP é um padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento do modo de agente e de sua interação com contexto, modelo de linguagem grande (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

O acesso ocorre por meio de um servidor MCP, que funciona como uma ponte entre o agente e as ferramentas e serviços externos, como APIs (interfaces de programação de aplicações) existentes ou ferramentas locais em pacotes npm. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp-server]**: oferece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece automação de navegador com o Playwright para navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, examine o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Observe que o servidor `github` já está configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada vem do modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]. Não é necessário instalar nada localmente: o VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicione o servidor MCP do Playwright

Adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**. O VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário em um Codespace sem ambiente gráfico.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, portanto o navegador geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pergunta se você confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir para confirmar que você confia no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciá-lo manualmente é opcional. Se você ignorar essas etapas, o VS Code iniciará o servidor e pedirá para você confiar nele quando o Copilot precisar das ferramentas no próximo exercício.

## Teste a filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. O agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumi-los, sem que você precise sair da conversa ou percorrer o aplicativo manualmente.

O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente interpreta elementos estruturados e rotulados, como botões, links e itens de lista, de maneira semelhante à tecnologia assistiva. Uma verificação funcional rápida também pode servir como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Use o servidor MCP do Playwright para abrir um navegador no aplicativo em execução em http://localhost:4321 e verificar a nova funcionalidade de filtragem de jogos:

1. Acesse a página de jogos e informe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme que a lista mostra somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista mostra somente jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme que os resultados correspondem aos dois.

Relate o que observar em cada etapa e indique qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará as etapas e apresentará os resultados. Compare o relatório com os critérios de aceitação da issue. Se algo estiver incorreto, faça perguntas ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira vez que usar o servidor MCP do Playwright, o Copilot talvez precise baixar um navegador. Se informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Publique a branch e crie um pull request

Depois de confirmar que a funcionalidade funciona, publique a branch `filtering-vscode` para que a equipe possa revisar as alterações.

1. Abra o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Você pode criar um pull request no github.com ou pela interface de linha de comando do GitHub (CLI). Como já está usando o GitHub Copilot, peça que ele encontre a issue relacionada e crie um PR associado a ela.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Na lista de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Selecione o link fornecido pelo Copilot e revise o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Nesta lição, você:

- aprendeu o que é o MCP e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente a filtragem no navegador antes de publicar.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para testar uma funcionalidade, mas o MCP também pode ampliar o Copilot de outras maneiras. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
