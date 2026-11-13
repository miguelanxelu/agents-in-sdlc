---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas dos agentes com pull requests (PRs) abertos, examinar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar controles de alternância, incluir essa funcionalidade faz sentido. Oriente o agente para que implemente o modo claro e o modo de alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Confira o status atual de todas as tarefas atribuídas ao agente de nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualização da documentação do código.
   - geração de APIs para modificar jogos.
   - adição do modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho.
   - configurar servidores do Model Context Protocol (MCP).
   - executar etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, que permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você examinou as tarefas ativas, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato dependerá do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no painel central de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a sessão indique que a configuração terminou e o trabalho começou.
3. Na caixa de diálogo **Steer active session while Copilot is working**, digite:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, que permita às pessoas selecionar o modo de exibição desejado.
   ```

   ![Tarefa do agente de nuvem com a caixa de diálogo para orientar a sessão ativa em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixe o Copilot trabalhar

O Copilot incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso levará alguns minutos. Aproveite o tempo para refletir sobre o que você aprendeu até aqui.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o painel central para monitorar e orientar as tarefas do agente de nuvem. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes tópicos:

- usou o painel de agentes para monitorar as tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, [itere sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
