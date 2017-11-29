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

# Aggiornamento degli spazi di lavoro

Il servizio di conversazione aggiunge e aggiorna regolarmente le funzioni. Mentre alcune di queste modifiche vengono applicate automaticamente allo spazio di lavoro, le modifiche che hanno un impatto maggiore richiedono un aggiornamento manuale.
{: shortdesc}

Se hai un'istanza che è stata creata prima del 3 febbraio 2017 e per il tuo spazio di lavoro è disponibile un aggiornamento, viene visualizzata l'icona di aggiornamento (![icona di aggiornamento](images/upgrade.png)) per indicare che è disponibile un aggiornamento. Seleziona questa icona per aprire la finestra di dialogo Aggiorna spazio di lavoro.

Quando aggiorni il tuo spazio di lavoro, nello strumento viene abilitata l'ultima versione dell'API e il riquadro "Provalo" inizia a utilizzare le funzioni più recenti.

Dopo aver aggiornato uno spazio di lavoro, non puoi ripristinarlo a una versione precedente.

## Aggiornamento del tuo spazio di lavoro
Per evitare di ostacolare le funzionalità nel tuo spazio di lavoro:

1.  [Duplica il tuo spazio di lavoro](configure-workspace.html#exporting-and-copying-workspaces).
2.  Aggiorna lo spazio di lavoro duplicato.
3.  Verifica la presenza di problemi nello spazio di lavoro aggiornato.

Quando termini di verificare i problemi, applica l'aggiornamento alla tua applicazione modificando la chiamata API di messaggio per utilizzare **03-03-2017** o successiva.

## Modifica nelle azioni Passa a
L'elaborazione delle azioni **Passa a** è cambiata con la release del 3 febbraio 2017.

In precedenza, se passavi alla condizione di un nodo e né tale nodo né uno dei suoi nodi peer aveva una condizione valutata come true, il sistema sarebbe passato al nodo di livello root e avrebbe cercato un nodo la cui condizione soddisfasse l'input. In alcune situazioni, questa elaborazione creava un loop che impediva l'avanzamento del dialogo.

Nel nuovo processo, se né il nodo di destinazione né i suoi peer vengono valutati come true, il turno di dialogo viene terminato. Qualsiasi risposta generata viene restituita all'utente mentre all'applicazione viene restituito un messaggio di errore:

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

Il successivo input utente viene gestito al livello root del dialogo.

Questo aggiornamento potrebbe cambiare il comportamento del tuo dialogo se hai delle azioni **Passa a** destinate a nodi le cui condizioni sono false.

Se vuoi ripristinare il vecchio modello di elaborazione, aggiungi un nodo peer finale con una condizione `true`. Nella risposta, utilizza un'azione **Passa a** da destinare alla condizione del primo nodo al livello root della tua struttura ad albero di dialogo.
