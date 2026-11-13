---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É comum que organizações tenham dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão de uma pessoa desenvolvedora. Assim, tarefas repetitivas podem ser delegadas, acelerando o desenvolvimento e liberando os desenvolvedores para se concentrarem em questões mais amplas.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- criar issues com escopo bem definido.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. Os prestadores de serviço que criaram a primeira versão do site não documentaram o código. Como primeiro passo, a equipe quer adicionar comentários de documentação do TypeScript (TSDoc) às funções exportadas do aplicativo.

A equipe de design também quer facilitar a descoberta de jogos. Ela quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para explorar a experiência e fazer testes de aceitação. A funcionalidade não precisa estar finalizada, mas está bloqueada por outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser adiadas. Atribua-as ao agente de nuvem do Copilot, que trabalhará de forma assíncrona enquanto você se dedica a outras tarefas. Depois, revise o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot][assign-issue]. O Copilot criará um pull request de rascunho para acompanhar o progresso, configurará um ambiente e começará o trabalho. Você pode examinar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo definido

O GitHub Copilot não é mágico: você ainda precisa pensar no problema. Mesmo tarefas aparentemente simples podem ter complexidades ocultas. Leia as [práticas recomendadas para definir tarefas para o agente de nuvem][cloud-agent-best-practices] e trate-o como um parceiro de programação com inteligência artificial: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inteligência artificial generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas. Se você já concluiu o exercício prático, viu `.github/copilot-instructions.md` em uso; caso contrário, examine os arquivos agora, antes de atribuir trabalho ao agente de nuvem. Assim, você poderá observar o efeito deles depois.

Abra estes arquivos na interface Web do GitHub ou em um Codespace:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `O código não tem documentação` ao agente de nuvem, observe se o pull request contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções e serão mencionadas novamente durante a revisão dos PRs.

## Configure o ambiente do agente de nuvem do Copilot

Criar código normalmente exige um ambiente específico e scripts de configuração. O agente de nuvem também precisa disso para executar tarefas como um engenheiro de software.

O agente usa [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [workflow de configuração especial][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. O workflow deste workshop já está configurado. Ele instala o Node.js e as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e configura o banco de dados SQLite migrado e preenchido usado pelo aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina apenas as permissões mínimas necessárias para estas etapas. O Copilot recebe um token próprio para suas operações.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, é necessária a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se a configuração funciona sem esperar uma execução do Copilot.

## Adicione documentação

Muitos projetos têm documentação desatualizada ou inexistente. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código e a integração de novos desenvolvedores. O Copilot pode criar documentação, por isso esta é uma boa tarefa para atribuir ao agente de nuvem. Você voltará depois para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `O código não tem documentação`.
6. No campo **Description**, digite:

   ```plaintext
   Nossa organização exige que funções e métodos tenham comentários de documentação TSDoc quando eles forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários que esclareçam o comportamento.
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request (PR), com um título semelhante a `[WIP]: O código não tem documentação`. Se ele ainda não aparecer, aguarde alguns minutos e atualize a página.
12. Aguarde o Copilot criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer. O Copilot está configurando o ambiente com o workflow descrito acima, analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Percorra a linha do tempo do PR até encontrar uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Examine a sessão em andamento e observe como o Copilot explora o código, entende o estado do projeto, planeja a abordagem e cria código.

O trabalho levará alguns minutos. Uma vantagem do agente de nuvem é executar tarefas de forma assíncrona, enquanto você se dedica a outras atividades. Aproveite esse recurso atribuindo outra tarefa e, em seguida, trabalhe em uma funcionalidade do aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Você pode dividir o trabalho: enquanto se dedica a uma tarefa, o agente de nuvem executa outra. A equipe de design quer uma seção de jogos relacionados na página de detalhes. Atribua essa solicitação ao Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Exiba jogos relacionados na página de detalhes do jogo`.
6. No campo **Description**, digite:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos, exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário (UX) e fazer testes de aceitação. Estes são os requisitos:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Exiba uma seção "Jogos relacionados" na página de detalhes do jogo usando o helper.
   - Trate o caso em que um jogo não tenha jogos relacionados.
   - Crie testes unitários para o novo helper.
   - Antes de criar o PR, confirme que todos os testes passam.
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, um símbolo 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![Emoji de olhos que indica que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente de nuvem está trabalhando na segunda solicitação. Como ele trabalha de forma semelhante a um engenheiro de software, não é necessário monitorá-lo continuamente; revise o resultado quando estiver pronto. Enquanto isso, explore os agentes personalizados.

## Resumo e próximos passos

Você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona e delegar tarefas de dívida técnica, novas funcionalidades ou migrações entre frameworks.

Você explorou estes tópicos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- criar issues com escopo bem definido.
- atribuir issues ao Copilot.

Enquanto o agente trabalha, explore como criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e instruções personalizadas, que foram apresentadas nos exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
