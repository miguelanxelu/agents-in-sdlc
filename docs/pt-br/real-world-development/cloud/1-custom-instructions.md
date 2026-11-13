---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações de contexto — disponibilize essas orientações. Os [arquivos de instruções][instruction-files] fornecem contexto para que o Copilot entenda não só *o que* fazer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos de instruções específicos para determinados caminhos.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, aqui você não fará uma comparação antes e depois do prompt. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, por isso é mais difícil demonstrar o efeito lado a lado. Você verá a influência das instruções mais adiante, ao revisar os pull requests criados pelo agente.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve conter comentários de documentação do TypeScript (TSDoc).
- O início de cada arquivo deve conter um bloco de comentários que descreva a finalidade do arquivo.

Use arquivos de instruções para fornecer ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos de codificação. Você pode especificar convenções, bibliotecas e os tipos de comentários que prefere. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter contexto relevante para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> Em um IDE, os arquivos de instruções orientam a geração de código no Copilot Chat, não as sugestões de conclusão de código nem as sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste workshop, mas o projeto de exemplo demonstra uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto, como descrição, estrutura e padrões globais de codificação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos do formato de código esperado e do que deve ser evitado.

Não há uma única maneira correta de criar arquivos de instruções ou de usar inteligência artificial. Faça experiências para descobrir o que funciona melhor no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Este projeto inclui instruções para tarefas como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa função de uma maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O percurso correspondente explica o recurso quando necessário.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório: o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações subordinadas para **GitHub Actions Workflows**. Essas instruções se aplicam às interações com o Copilot neste repositório.
3. Explore a pasta `.github/instructions`. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` define um glob relativo à raiz do repositório. Nesse caso, arquivos de teste TypeScript, como `**/*.test.ts`, correspondem ao padrão.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e consulte o final do arquivo. Observe os links para outras instruções, como `unit-tests.instructions.md`, e para arquivos do projeto. Assim, você pode dividir conjuntos de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicione um padrão ao repositório

Adicione uma regra para todo o projeto que exija comentários TSDoc e um cabeçalho de comentário no início dos arquivos. O agente de nuvem usará essas instruções quando trabalhar nas issues atribuídas mais adiante.

Antes de editar, crie uma branch. O agente de nuvem lê as instruções da branch de destino da issue, mas você fará o commit das alterações de forma organizada.

1. No terminal do Codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

O arquivo `.github/copilot-instructions.md` fornece orientações gerais sobre o projeto. Registre nele os padrões de codificação para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela ainda não exige documentação no código, por isso o helper gerado não tinha comentários.
3. Logo abaixo dos padrões existentes, adicione estas linhas para exigir cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser definidas no repositório em `.github/copilot-instructions.md` ou em arquivos `*.instructions.md` específicos. O arquivo do repositório é adequado para padrões gerais, como a regra de documentação que você acabou de adicionar.

## Faça commit, envie e mescle as instruções

O agente de nuvem lê as instruções da branch de destino da issue. No próximo exercício, o agente criará uma branch a partir de `main`; portanto, as instruções precisam estar em `main` para serem aplicadas.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem usará essas instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore as etapas da branch e faça commit diretamente em `main`. A branch foi incluída para refletir o fluxo de trabalho de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções e adicionou um padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem criar nas próximas atividades.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

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
