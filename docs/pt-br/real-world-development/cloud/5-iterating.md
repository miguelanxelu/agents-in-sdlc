---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste workshop, você atribuiu várias tarefas ao GitHub Copilot para melhorar a experiência do usuário e adicionar funcionalidades: documentar o código, criar uma seção de jogos relacionados e implementar recursos de acessibilidade, como os modos de alto contraste e claro. Revise as alterações e, se necessário, envie comentários ao Copilot para melhorar o resultado.

### Cenário

Os fundamentos do desenvolvimento de software e do DevOps não mudam com a inteligência artificial generativa. Revise o código gerado pelo GitHub Copilot e siga o processo DevOps habitual. Examine as sugestões para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão do restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente efêmero separado.
- Uma pessoa precisa aprovar os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores do Model Context Protocol (MCP).

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem: o que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot está bloqueado por um firewall. Isso é esperado, pois o acesso do Copilot a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, [personalize ou desabilite o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Adicionar documentação ausente**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários de documentação do TypeScript (TSDoc) e outra documentação. As alterações exatas podem variar. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário, conforme as instruções personalizadas examinadas no Exercício 2.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Confirme que os workflows foram adicionados à seção de verificações do PR e que as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para atender às solicitações. Como os resultados são não determinísticos, não há um prompt único obrigatório. Você pode pedir, por exemplo, para:

- adicionar um cabeçalho com uma breve descrição no início de cada arquivo de código.
- adicionar comentários de documentação do TypeScript (TSDoc) a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com outra pessoa. Use uma das ideias acima ou outra solicitação.
2. Selecione **View Session** para acompanhar o trabalho do Copilot e observar a nova sessão.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, confirme que um novo commit foi adicionado ao PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, marque o PR de rascunho como pronto para revisão e mescle-o na branch principal.

![Opção para marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Agora revise o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Exiba jogos relacionados na página de detalhes do jogo**.
4. Selecione **Files changed** e revise o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Verifique se há um indicador de que workflows aguardam aprovação.
7. Se houver, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Confirme que os workflows foram adicionados às verificações do PR e que as verificações do aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** teste manualmente a funcionalidade no Codespace. Abra um terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a funcionalidade de jogos relacionados. Você pode pedir outras alterações com comentários na guia **Conversation**; o Copilot iniciará uma nova sessão. Por exemplo, solicite que altere a quantidade de jogos relacionados ou adicione cabeçalhos e comentários TSDoc. Observe que essa issue foi atribuída antes da atualização das instruções personalizadas.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade criados com o agente personalizado. O PR deve incluir o modo de alto contraste do Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Adicione o modo de alto contraste ao site**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão.

4. Selecione **Files changed** para revisar o código.
5. Examine a implementação, principalmente:
   - os controles de interface para alternar entre modos.
   - o uso do armazenamento local para preservar as preferências.
   - as alterações de CSS (Cascading Style Sheets) ou estilo para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA (Accessible Rich Internet Applications) e navegação pelo teclado.
   - o código JavaScript/TypeScript que controla a alternância entre modos.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de que workflows aguardam aprovação.
8. Se houver, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Confirme que os workflows foram adicionados às verificações do PR e que as verificações do aplicativo Astro foram aprovadas.
10. **Opcional:** teste manualmente os recursos de acessibilidade no Codespace. Abra um terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade orientou o Copilot a implementar os recursos seguindo práticas de acessibilidade. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional: continue delegando

O agente de nuvem é útil para trabalhar em itens reais do backlog enquanto você se dedica a outras tarefas. Crie mais issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizadas** do repositório para observar o efeito deles nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar, explore outros percursos:

- [Percurso do VS Code](../../vscode/): use o modo de agente do Copilot Chat e a integração MCP no ambiente de desenvolvimento integrado (IDE).
- [Percurso da interface de linha de comando (CLI)](../../cli/): entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
