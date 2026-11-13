---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório para fornecer instruções, práticas recomendadas, padrões de código e conhecimento de domínio. As equipes podem transformar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e explicam a um agente de inteligência artificial como executar tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem coordenar skills e até outros agentes. Neste exercício, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Como parte do trabalho, ele poderá usar skills, como uma para o fluxo de pull requests ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer trabalho com inteligência artificial, teste e itere para encontrar o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, implemente a funcionalidade rapidamente usando um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

O projeto já inclui um agente personalizado para acessibilidade. Revise o arquivo para entender como ele orientará o Copilot.

1. Volte ao Codespace e, em um terminal, crie uma nova branch a partir de `main` para o trabalho de acessibilidade. Assim, o PR de filtragem do exercício anterior permanecerá separado:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter em YAML (YAML Ain't Markup Language) com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código em HTML (HyperText Markup Language), CSS (Cascading Style Sheets) e JavaScript.
   - problemas comuns e erros a evitar.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` na lista de agentes, na parte inferior da visualização Copilot Chat. Selecione um agente personalizado para aplicar as instruções e ferramentas dele à sessão.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indica o que é necessário.

1. Na lista de agentes da visualização Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Lista de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista de agentes na parte inferior da visualização Chat. Por padrão, ela mostra o agente **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Adicione um modo de alto contraste ao site. Deve haver um controle que a pessoa usuária possa ativar para habilitar o alto contraste, e a configuração deve persistir após recarregar a página usando o armazenamento local do navegador.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. Observe o indicador **Files changed**, que atualiza a lista de arquivos modificados pelo agente.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, como editar componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes tópicos:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [monitore e oriente o trabalho do agente][next-lesson], revise as alterações conforme elas acontecem e adicione um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no ambiente de desenvolvimento integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar a funcionalidade com o servidor do Model Context Protocol (MCP) do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
