---
slug: pt-br/real-world-development/app
title: "Aplicativo GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** é um aplicativo para desktop baseado no Copilot CLI que reúne o desenvolvimento orientado por agentes em um espaço de trabalho único e focado. Ele adiciona sessões paralelas de agentes, modos de sessão alternáveis, canvases compartilhados e gerenciamento nativo de issues e pull requests do GitHub, incluindo o **Agent Merge**, que conduz um pull request por rebases, feedback de revisão, correções de integração contínua (CI) e mesclagem.

O workshop acompanha um fluxo contínuo de trabalho na Tailspin Toys:

1. Prepare o projeto, instale o aplicativo, conecte o repositório e explore o espaço de trabalho e o backlog predefinido.
2. Faça uma alteração específica nas avaliações por estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo **Plan**, implemente-a no modo **Autopilot** e revise-a no modo **Interactive**.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor do Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise toda a alteração de filtragem e use o Agent Merge no segundo PR.
9. Verifique se os canvases estão disponíveis; depois, use o Database Explorer e crie e teste um canvas de triagem vinculado ao repositório. Se os recursos necessários não estiverem disponíveis, registre o impedimento e use a alternativa somente leitura ou continue para a recapitulação.

Para manter o foco do workshop, você criará dois PRs: um para avaliações por estrelas e outro para filtragem, com as atualizações de instruções e da skill, o perfil de QA e os testes. Comece cada um a partir de `main` atualizado. O fluxo de filtragem e qualidade compartilha uma sessão, uma worktree e uma branch para que você possa aproveitar seu trabalho à medida que explora cada ferramenta. O exercício final de canvas permanece em sua própria sessão para que você se concentre em criar e testar a superfície compartilhada sem repetir o fluxo de PR.

## Lições

| Lição | Tópico | Descrição |
|--------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Instale o Node.js e crie sua cópia do projeto Tailspin Toys |
| [1. Instale o aplicativo Copilot][ex1] | Configuração | Instale o aplicativo, conecte seu projeto e conheça o espaço de trabalho |
| [2. Adicione avaliações por estrelas: uma vitória rápida][ex2] | Primeira alteração | Exiba as avaliações existentes e a alternativa para valores nulos e mescle o PR 1 |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje a funcionalidade a partir da issue, implemente-a com o Autopilot e revise-a no modo Interactive |
| [4. Oriente o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize instruções e aplique-as à filtragem |
| [5. Personalize e use uma skill quality-checks][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Valide a funcionalidade com o MCP do Playwright][ex6] | Observação no navegador | Configure o MCP em Customize e examine o comportamento da filtragem |
| [7. Crie e use um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna evidências finais de verificação |
| [8. Crie e mescle o PR da funcionalidade][ex8] | Revisão e mesclagem | Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois use o Agent Merge no segundo PR |
| [9. Explore e crie canvases][ex9] | Colaboração | Use o Database Explorer e crie e teste um canvas de triagem vinculado ao repositório |
| [10. Recapitulação e próximos passos][ex10] | Resumo | Revise o fluxo, os artefatos e outros recursos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Um computador com **macOS, Linux ou Windows**
- [ ] O [Git instalado][install-git] no computador

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Como o aplicativo Copilot é executado no seu computador, e não em um Codespace, [a lição de pré-requisitos][ex0] orienta você na instalação do Node.js e na criação da sua cópia do projeto antes da instalação do aplicativo.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**. Essa política é diferente da política do Copilot CLI.

## Começar

**[Comece pelos pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
