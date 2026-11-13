---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Os [arquivos de instruções][instruction-files] fornecem orientações sobre como gerar código, incluindo o contexto do projeto, as diretrizes de codificação e os padrões de documentação.

Neste exercício, você vai:

- explorar como o contexto do projeto e as diretrizes chegam ao Copilot por meio de instruções do repositório e de arquivos específicos para determinados caminhos.
- gerar a primeira parte da camada de dados para filtragem — um helper de editoras — usando as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- repetir o mesmo prompt e observar o código gerado com o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a tendência de mudança após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve conter comentários de documentação do TypeScript (TSDoc).
- O início de cada arquivo deve conter um bloco de comentários que descreva a finalidade do arquivo.

Use arquivos de instruções para fornecer ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot. Elas podem especificar convenções de codificação, bibliotecas e os tipos de comentários preferidos. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório, com contexto e orientações gerais sobre o projeto.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas, como TypeScript, Astro, componentes de interface e testes.

> [!NOTE]
> No IDE, os arquivos de instruções orientam a geração de código no Copilot Chat, não as conclusões de código nem as sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado em informações gerais do projeto, como descrição, estrutura e padrões globais de codificação.
- Use arquivos `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos do formato de código esperado e do que deve ser evitado.

Não há uma única maneira correta de criar arquivos de instruções ou de usar inteligência artificial. Faça experiências para descobrir o que funciona melhor.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter um conjunto sólido de arquivos de instruções. Este projeto inclui instruções para [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa função de uma maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Leia os arquivos incluídos no repositório: o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações subordinadas para **GitHub Actions Workflows**.
3. Explore `.github/instructions`. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` define um glob relativo à raiz do repositório. Arquivos de teste TypeScript, como `**/*.test.ts`, correspondem a esse padrão.
5. Observe as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e exemplos no projeto. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário.

## Crie uma branch para as alterações

Crie uma branch para manter as alterações separadas.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício.

Envie um prompt com o arquivo de instruções atual. Depois, atualize as instruções e envie o mesmo prompt novamente para comparar o resultado.

1. Feche as guias de editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na lista de agentes da visualização Chat, selecione **Agent** para permitir que o Copilot altere arquivos.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e a ID de cada uma. Aplique as alterações ao arquivo.
   ```

5. O Copilot explorará o projeto e aplicará alterações, normalmente ao helper e aos testes.
6. Observe que a função proposta é tipada, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode adicionar comentários mesmo sem essa orientação. Isso não é um problema; o objetivo é observar se as instruções tornam esse comportamento mais consistente.

## Adicione um padrão para todo o repositório

Adicione orientações gerais a `.github/copilot-instructions.md` para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, perto da linha 27. Observe que a seção ainda não contém regras para documentação no código.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções do repositório são adequadas para padrões gerais. Use arquivos `*.instructions.md` para orientações específicas de linguagens, tipos de arquivo ou tarefas.

## Repita o prompt e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Abra `src/lib/publishers.ts` para direcionar o Copilot ao arquivo correto.
3. Envie o mesmo prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e a ID de cada uma. Aplique as alterações ao arquivo.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras da plataforma de financiamento coletivo da Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com suas IDs e seus nomes.
    *
    * @param db - O cliente de banco de dados do Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editora.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são controlados pelo Git como qualquer outro arquivo. Faça commit e envie a branch ao repositório.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Execute `git status` para confirmar que as instruções e o helper estão presentes.
3. Prepare e faça commit das instruções e dos arquivos alterados pelo Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas, como testes, inclua esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Associe a branch enviada à issue sobre os padrões de documentação. As ferramentas do GitHub já estão conectadas ao projeto e o Copilot pode encontrar a issue, criar o pull request (PR) e mesclá-lo.

> [!NOTE]
> Esta é a primeira atividade em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub. Talvez o VS Code solicite autenticação na primeira vez que o Copilot usar uma delas.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que encontre a issue das instruções e crie um PR a partir da branch atual, descrevendo também a base do helper de editoras:

   ```plaintext
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, indique que o PR fecha essa issue e mencione que também adicionamos a base do helper de editoras para o próximo trabalho de filtragem.
   ```

4. Aguarde o Copilot encontrar a issue e criar o PR.
5. Quando solicitado, revise a ação do GitHub e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot que o mescle e atualize a branch local:

   ```plaintext
   Mescle o PR em main. Depois, volte para main localmente e obtenha o código mais recente para deixar a cópia atualizada.
   ```

7. Quando solicitado, revise as ações do GitHub e os comandos de shell e selecione **Allow** se estiver de acordo.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o contexto do projeto e as diretrizes chegam ao Copilot e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um padrão para todo o repositório a `.github/copilot-instructions.md`.
- repetir o prompt e observar o código proposto com o novo padrão.
- fazer commit das instruções e da base do helper de editoras em `main` para o próximo exercício.

Em seguida, use essas instruções no [modo de agente][next-lesson] para adicionar uma funcionalidade à base de código.

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
