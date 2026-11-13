---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar diffs, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também um controle para o modo claro. Em vez de iniciar uma nova conversa, oriente a sessão existente para ampliar o trabalho.

## Monitore o progresso do Copilot

A visualização Copilot Chat mostra as etapas executadas pelo agente: arquivos examinados, comandos executados e edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta, leitura e edição de arquivo e comando de terminal solicitado.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra também o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados na sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente pausará e exibirá **Allow**. Aprove apenas os comandos que você considerar seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final que resume as alterações.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar um controle do modo claro. Como você continuará na mesma sessão, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o uso do armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Muito bem! Agora adicione um controle do modo claro ao lado do controle de alto contraste. O modo claro deve seguir o mesmo padrão: persistir no armazenamento local e ficar disponível na mesma área da interface. A pessoa usuária deve poder ativar o modo de alto contraste e o modo claro de forma independente.
   ```

2. O agente continuará o trabalho, examinará as alterações existentes e proporá adições para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de uma maneira que você não queira, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer a solicitação.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para pedir ajustes, corrigir o rumo ou ampliar o escopo. Por exemplo, peça para simplificar o CSS usando variáveis em vez de duplicar seletores, integrar o modo claro ao painel de configurações existente em vez de adicionar um terceiro controle ou incluir atalhos de teclado para os dois controles.

## Revise o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve mostrar componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente estão presentes, incluindo atributos ARIA (Accessible Rich Internet Applications), navegação pelo teclado, HTML (HyperText Markup Language) semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local durante o workshop.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
