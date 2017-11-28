---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Fazendo upgrade de áreas de trabalho

O serviço de Conversa regularmente inclui e atualiza recursos. Embora algumas dessas mudanças sejam aplicadas automaticamente para sua área de trabalho, as atualizações que têm um grande impacto requerem uma atualização manual.
{: shortdesc}

Se você tiver uma instância que foi criada antes de 3 de fevereiro de 2017 e sua área de trabalho tiver uma atualização disponível, o ícone de atualização (![ícone de atualização](images/upgrade.png)) será exibido para indicar que um upgrade está disponível. Selecione este ícone para abrir o diálogo da área de trabalho Upgrade.

Ao fazer upgrade da área de trabalho, a versão mais recente da API é ativada na ferramenta e o painel "Experimente" começa a usar os recursos mais recentes.

Depois de atualizar uma área de trabalho, você não pode reverter sua área de trabalho para uma versão anterior.

## Atualizando sua área de trabalho
Para evitar entraves de funcionalidade em sua área de trabalho:

1.  [Duplique sua área de trabalho](configure-workspace.html#exporting-and-copying-workspaces).
2.  Atualize a área de trabalho duplicada
3.  Teste se há problemas na área de trabalho submetida a upgrade.

Ao concluir o teste em busca de problemas, aplique o upgrade ao aplicativo mudando a chamada API da mensagem para usar **03-02-2017** ou posterior.

## Mudança nas ações Ir para
O processamento de ações **Ir para** mudou com a liberação de 3 de fevereiro de 2017.

Anteriormente, se você pulasse para a condição de um nó e nem o nó nem qualquer um de seus nós de mesmo nível tivessem uma condição que foi avaliada como verdadeira, o sistema pularia para o nó de nível raiz e procuraria por um nó cuja condição correspondesse à entrada. Em algumas situações, esse processamento criou um loop, o que impediu o progresso do diálogo.

Sob o novo processo, se nem o nó de destino nem seus peers forem avaliados como verdadeiros, a rodada do diálogo será finalizada. Qualquer resposta que foi gerada será retornada para o usuário e uma mensagem de erro será retornada para o aplicativo:

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

A próxima entrada do usuário é manipulada no nível raiz do diálogo.

Essa atualização pode mudar o comportamento de seu diálogo se você tiver ações **Ir para** destinadas a nós cujas condições sejam falsas.

Se você deseja restaurar o modelo de processamento antigo, inclua um nó de mesmo nível final com uma condição `true`. Na resposta, use uma ação **Ir para** destinada à condição do primeiro nó no nível raiz de sua árvore de diálogos.
