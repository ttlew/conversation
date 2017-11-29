---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-30"

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

# Utilizzo delle conversazioni
{: #logs_convo}

Per aprire un elenco di interazioni tra gli utenti e il tuo robot, seleziona **Conversazioni utente** nella barra di navigazione. Se **Conversazioni utente** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.
{: shortdesc}

Quando apri la pagina **Conversazioni utente**, la vista predefinita elenca i risultati per l'ultimo giorno, con i risultati più recenti per primi. Sono disponibili i valori di intenti principali (#intent) e qualsiasi entità riconosciuta (@entity) utilizzati nel messaggio e il testo del messaggio. Per gli intenti che non sono riconosciuti, il valore mostrato è *Irrilevante*. Se un'entità non è riconosciuta o non è stata fornita, il valore mostrato è *Nessuna entità trovata*.
![Pagina predefinita Log](images/logs_page1.png)

## Limiti di log
{: #log-limits}

La durata prevista per la conservazione dei messaggi di chat dipende dal tuo piano di servizio {{site.data.keyword.conversationshort}}:

  Piano di servizio                    | Conservazione log di chat
  ------------------------------------ | ------------------------------------
  Premium                              | Ultimi 90 giorni
  Standard                             | Ultimi 30 giorni
  Gratuito                             | Ultimi 7 giorni

## Filtro di espressioni

È importante notare che la pagina **Conversazioni utente** visualizza il numero totale di *espressioni* tra gli utenti e il tuo bot. Un'espressione è un singolo messaggio che l'utente invia al bot. Ogni conversazione può essere composta da più espressioni. Pertanto, il numero di risultati nella pagina **Conversazioni utente** è diverso dal numero di conversazioni mostrato nella pagina [Panoramica](logs_oview.html).

Puoi filtrare le espressioni in base ai valori *Cerca istruzioni utente*, *Intenti*, *Entità* e *Ultimi* n *giorni*:

*Cerca istruzioni utente* - Immetti una parola nella barra di ricerca. Vengono cercati gli input degli utenti ma non le risposte del bot.

*Intenti* - Seleziona il menu a discesa e immetti un intento nel campo di input o scegli una voce dall'elenco. Puoi selezionare più di un intento, che filtra i risultati utilizzando uno qualsiasi degli intenti selezionati, incluso *Irrilevante*.

![Menu a discesa Intenti](images/intents_filter.png)

*Entità* - Seleziona il menu a discesa e immetti un nome entità nel campo di input o scegli una voce dall'elenco. Puoi selezionare più di un'entità, che filtra i risultati in base a una qualsiasi entità selezionata. Se filtri per intento *ed* entità, i tuoi risultati includeranno i messaggi che hanno entrambi i valori. Puoi anche filtrare i risultati con *Nessuna entità trovata*.

![Menu a discesa Entità](images/entities_filter.png)

L'aggiornamento delle espressioni può richiedere del tempo. Attendi almeno 30 minuti dopo l'interazione dell'utente con il tuo bot prima di tentare di filtrare quel contenuto.

## Visualizzazione di una singola espressione
Puoi espandere ciascuna voce di espressione per vedere cosa ha detto l'utente nell'intera conversazione e come ha risposto il bot. Per farlo, seleziona **Apri conversazione**. Vieni portato automaticamente all'espressione che hai selezionato in quella conversazione.

![Pannello Apri conversazione](images/open_convo.png)

Puoi quindi scegliere di mostrare le classificazioni per l'espressione che hai selezionato.

![Pannello Apri conversazione con le classificazioni](images/open_convo_classes.png)

## Correzione di un intento

1.  Per correggere un intento, seleziona l'icona di modifica ![Modifica](images/edit_icon.png) accanto all'#intent scelto.
1.  Dall'elenco fornito, seleziona l'intento corretto per questo input.
    - Inizia a scrivere nel campo di immissione e l'elenco di intenti verrà filtrato.
    - Puoi anche scegliere **Contrassegna come irrilevante** da questo menu. (Per ulteriori informazioni, vedi "[Contrassegna come irrilevante](intents.html#mark-irrelevant)".) In alternativa, puoi scegliere **Non addestrare sull'intento**, che non salva questa espressione come esempio per l'addestramento.

    ![Seleziona intento](images/select_intent.png)
1.  Seleziona **Salva**.

    ![Salva intento](images/save_intent.png)

## Aggiunta di un valore o un sinonimo di entità

1.  Per aggiungere un valore o un sinonimo di entità, seleziona l'icona di modifica ![Modifica](images/edit_icon.png) accanto all'@entity scelta.
1.  Seleziona **Aggiungi entità**.

    ![Aggiungi entità](images/add_entity.png)
1.  Ora, seleziona una parola o una frase nell'input utente sottolineato.

    ![Seleziona entità](images/select_entity.png)
1.  Scegli un'entità a cui la frase evidenziata verrà aggiunta come valore.
    - Inizia a scrivere nel campo di immissione e l'elenco di entità e valori verrà filtrato.
    - Per aggiungere la frase evidenziata come sinonimo di un valore esistente, scegli "@entity:value" dall'elenco a discesa.

    ![Aggiungi parola di entità](images/add_entity_word.png)
1.  Seleziona **Salva**.

    ![Salva entità](images/add_entity_save.png)
