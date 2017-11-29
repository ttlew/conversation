---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-21"

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

# Modifica di un dialogo attraverso l'API

L'API REST {{site.data.keyword.conversationshort}} supporta la modifica del tuo dialogo a livello di programmazione, senza utilizzare lo strumento {{site.data.keyword.conversationshort}}. Puoi utilizzare l'API /dialog_nodes per creare, eliminare o modificare i nodi di dialogo. 

Ricorda che il dialogo è una struttura ad albero di nodi interconnessi e che per poter essere valido deve essere conforme a determinate regole. Ciò significa che qualsiasi modifica apportata a un nodo di dialogo potrebbe avere effetti a cascata su altri nodi o sulla struttura del tuo dialogo. Prima di utilizzare l'API /dialog_nodes per modificare il tuo dialogo, assicurati di capire in che modo le modifiche influenzeranno il resto del dialogo.

Un dialogo valido soddisfa sempre i seguenti criteri:

- Ogni nodo di dialogo ha un ID univoco (la proprietà `dialog_node`).
- Un nodo figlio è consapevole del suo nodo padre (la proprietà `parent`). Tuttavia, un nodo padre non è consapevole dei suoi nodi figlio.
- Un nodo è consapevole del suo immediato elemento di pari livello precedente, se presente (la proprietà `previous_sibling`). Ciò significa che tutti i nodi di pari livello di un determinato padre formano un elenco collegato, dove ogni nodo punta al nodo precedente.
- Un solo figlio di un determinato padre può essere il primo nodo di pari livello (il che significa che il suo `previous_sibling` è null).
- Un nodo non può puntare a un elemento di pari livello precedente che è figlio di un padre diverso.
- Due nodi non possono puntare allo stesso elemento di pari livello precedente.
- Un nodo può specificare un altro nodo da eseguire successivamente (la proprietà `next_step`).
- Un nodo non può essere il suo proprio padre o elemento di pari livello.
- Un nodo di tipo `response_condition` non può avere nodi figlio.

I seguenti esempi mostrano come varie modifiche potrebbero comportare delle modifiche a cascata:

## Creazione di un nodo
{: #create-node}

Considera la semplice struttura ad albero di dialogo riportata di seguito:

![Dialogo di esempio](images/dialog_api_1.png)

Possiamo creare un nuovo nodo effettuando una richiesta POST a /dialog_nodes col il seguente corpo:

```json
{
  "dialog_node": "node_8"
}
```

Il dialogo adesso sarà simile al seguente:

![Dialogo di esempio 2](images/dialog_api_2.png)

Poiché **node_8** è stato creato senza specificare un valore per `parent` o `previous_sibling`, è ora il primo nodo nel dialogo. Nota che, oltre a creare **node_8**, il servizio ha anche modificato **node_1** in modo che la sua proprietà `previous_sibling` punti al nuovo nodo.

Puoi creare un nodo in un'altra parte nel dialogo specificando l'elemento padre e quello di pari livello:

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

I valori che specifichi per `parent` e `previous_node` devono essere validi:

- Entrambi i valori devono fare riferimento a nodi esistenti.
- Il padre specificato deve essere uguale al padre dell'elemento di pari livello precedente (o `null`, se l'elemento di pari livello precedente non ha un padre).
- Il padre non può essere un nodo di tipo `response_condition`.

Il dialogo risultate è simile al seguente:

![Dialogo di esempio 3](images/dialog_api_3.png)

Oltre a creare **node_9**, il servizio aggiorna automaticamente la proprietà `previous_sibling` di *node_6* in modo che punti al nuovo nodo.

## Spostamento di un nodo in un padre diverso
{: #change-parent}

Spostiamo **node_5** in un padre diverso utilizzando il metodo POST /dialog_nodes/node_5 con il seguente corpo:

```json
{
  "parent": "node_1"
}
```

Il valore specificato per `parent` deve essere valido:
- Deve fare riferimento a un nodo esistente.
- Non deve fare riferimento al nodo che viene modificato (un nodo non può essere il suo proprio padre).
- Non deve fare riferimento a un discendente del nodo che viene modificato.
- Non deve fare riferimento a un nodo di tipo `response_condition`.

Questo produce la seguente struttura modificata:

![Dialogo di esempio 4](images/dialog_api_4.png)

Qui sono successe diverse cose:
- Quando **node_5** è stato spostato nel suo nuovo padre, **node_7** è andato con lui (perché il valore `parent` per **node_7** non è cambiato). Quando sposti un nodo, tutti i discendenti di tale nodo restano con lui.
- Poiché non abbiamo specificato un valore `previous_sibling` per **node_5**, questo è adesso il primo nodo di pari livello sotto **node_1**.
- La proprietà `previous_sibling` di **node_4** è stata aggiornata a `node_5`.
- La proprietà `previous_sibling` di **node_9** è stata aggiornata a `null`, perché è adesso il primo elemento di pari livello sotto **node_2**.

## Risequenza degli elementi di pari livello
{: #change-sibling}

Adesso rendiamo **node_5** il secondo elemento di pari livello anziché il primo. Possiamo farlo utilizzando il metodo POST /dialog_nodes/node_5 con il seguente corpo:

```json
{
  "previous_sibling": "node_4"
}
```

Quando modifichi `previous_sibling`, il nuovo valore deve essere valido:
- Deve fare riferimento a un nodo esistente
- Non deve fare riferimento al nodo che viene modificato (un nodo non può essere il suo proprio elemento di pari livello)
- Deve fare riferimento al figlio dello stesso padre (tutti gli elementi di pari livello devono avere lo stesso padre)

La struttura cambia come segue:

![Dialogo di esempio 5](images/dialog_api_5.png)

Nota che ancora una volta **node_7** resta con il proprio padre. Inoltre, **node_4** viene modificato in modo che il suo `previous_sibling` sia `null`, perché è ora il primo elemento di pari livello.

## Eliminazione di un nodo
{: #delete-node}

Adesso, eliminiamo **node_1** utilizzando il metodo DELETE /dialog_nodes/node_1.

Il risultato è il seguente:

![Dialogo di esempio 6](images/dialog_api_6.png)

Nota che **node_1**, **node_4**, **node_5** e **node_7** sono stati tutti eliminati. Quando elimini un nodo, vengono eliminati anche tutti i discendenti di tale nodo. Pertanto, se elimini un nodo di base, in realtà elimini un intero ramo della struttura ad albero del dialogo. Qualsiasi altro riferimento al nodo eliminato (ad esempio, i riferimenti `next_step`) viene modificato in `null`.

Inoltre, **node_2** viene aggiornato per puntare a **node_8** come suo nuovo elemento di pari livello precedente.

## Ridenominazione di un nodo
{: #rename-node}

Infine, rinominiamo **node_2** utilizzando il metodo POST /dialog_nodes/node_2 con il seguente corpo:

```json
{
  "dialog_node": "node_X"
}
```

![Dialogo di esempio 7](images/dialog_api_7.png)

La struttura del dialogo non è cambiata, ma ancora una volta più nodi sono stati modificati per riflettere il nome modificato:

- Le proprietà `parent` di **node_9** e **node_6**
- La proprietà `previous_sibling` di **node_3**

Viene modificato anche qualsiasi altro riferimento al nodo eliminato (ad esempio, i riferimenti `next_step`).
