---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-25"

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

# Definizione di intenti

Gli ***Intenti*** sono scopi o obiettivi espressi nell'input di un cliente, come rispondere a una domanda o elaborare un pagamento di una fattura. Riconoscendo l'intento espresso nell'input di un cliente, il servizio {{site.data.keyword.conversationshort}} può scegliere il flusso di dialogo corretto per rispondere.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VG0YykNcfv8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Limiti di intenti
{: #intent-limits}

Il numero di intenti ed esempi che puoi creare dipende dal tuo piano di servizio {{site.data.keyword.conversationshort}}:

| Piano di servizio    | Intenti per spazio di lavoro | Esempi per spazio di lavoro |
|------------------|----------------------:|-----------------------:|
| Standard/Premium |                 2.000 |                 25.000 |
| Lite             |                    25 |                 25.000 |

## Creazione di intenti
{: #creating-intents}

Utilizza lo strumento {{site.data.keyword.conversationshort}} per creare gli intenti.

1.  Nello strumento {{site.data.keyword.conversationshort}}, apri il tuo spazio di lavoro e seleziona la scheda **Intenti** nella barra di navigazione. Se **Intenti** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.
1.  Seleziona **Crea nuovo**.
1.  Nel campo **Nome intento**, immetti un nome descrittivo per l'intento.
    - Il nome intento può contenere lettere (in Unicode), caratteri di sottolineatura, trattini e punti.
    - Il nome non può contenere `..` o qualsiasi altra stringa di soli punti.
    - I nomi intento non possono contenere spazi e non devono superare i 128 caratteri. Di seguito sono riportati degli esempi di nomi intento:
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    Gli strumenti includono automaticamente il carattere `#` nel nome dell'intento, per cui non dovrai aggiungerne uno.
    {: tip}

    Puoi selezionare **Crea** per salvare il tuo nome intento senza aggiungere degli esempi. Puoi anche selezionare il campo Esempio utente o utilizzare il tasto Tab per andare avanti e aggiungere esempi.

1.  Nel campo **Esempio utente**, immetti il testo di un esempio utente per l'intento. Un esempio può essere qualsiasi stringa composta da un massimo di 1024 caratteri. Quelli che seguono possono essere degli esempi per l'intento `#pay_bill`:
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    Se hai definito o prevedi di definire entità che corrispondono a questo intento, fai riferimento alle entità o ai loro sinonimi associati in alcuni degli esempi. In questo modo consenti di stabilire una relazione tra l'intento e le entità.

    > **Importante:** i nomi intento e il testo di esempio possono essere esposti negli URL quando un'applicazione interagisce con il servizio. Non includere informazioni sensibili o personali in queste risorse.

    Premi Invio o seleziona **+** per salvare l'esempio.
1.  Ripeti lo stesso processo per aggiungere altri esempi. Puoi passare da un esempio all'altro. Fornisci almeno 5 esempi per ogni intento. Più esempi fornisci, più accurata sarà la tua applicazione.

    ![Schermata che mostra la definizione dell'intento](images/define_intent.png)
1.  Quando hai finito di aggiungere gli esempi, seleziona **Fatto** per terminare la creazione dell'intento.

### Risultati

L'intento creato viene aggiunto alla scheda Intenti e il sistema comincia ad addestrarsi sui nuovi dati.

## Modifica degli intenti

Puoi selezionare qualsiasi intento nell'elenco per aprirlo per la modifica. Puoi apportare le seguenti modifiche:

- Rinominare l'intento.
- Eliminare l'intento.
- Aggiungere, modificare o eliminare gli esempi.
- Spostare un esempio in un intento diverso.

Puoi passare da un nome intento a ogni esempio e, se lo desideri, modificare gli esempi.

Per spostare un esempio, seleziona l'esempio selezionando la casella di spunta e seleziona quindi **Sposta in**.

  ![Schermata che mostra come spostare un esempio](images/move_example.png)

## Importazione di intenti ed esempi

Se hai un numero elevato di intenti ed esempi, è più facile importarli da un file CSV (comma-separated value) anziché definirli uno ad uno nello strumento {{site.data.keyword.conversationshort}}.

1.  Raccogli gli intenti ed esempi in un file CSV o esportali da un foglio di calcolo in un file CSV. Il formato richiesto per ogni riga nel file è il seguente:

    ```
    <example>,<intent>
    ```
    {: screen}

    dove `<example>` è il testo di un esempio utente e `<intent>` è il nome dell'intento che vuoi che corrisponda all'esempio. Ad esempio:

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    > **Importante:** salva il file CSV con la codifica UTF-8 e nessun contrassegno di ordine di byte (BOM). 

1.  Nello strumento {{site.data.keyword.conversationshort}}, apri il tuo spazio di lavoro e seleziona la scheda **Intenti** nella barra di navigazione. Se **Intenti** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.

1.  Seleziona ![Importa](images/importGA.png) e trascina un file oppure seleziona un file dal tuo computer. Il file viene convalidato e importato e il sistema comincia ad addestrarsi sui nuovi dati.

    > **Importante:** la dimensione massima del file CSV è 10 MB. Se il tuo file CSV è più grande, potresti suddividerlo in più file e importarli separatamente. 

### Risultati

Puoi visualizzare gli intenti importati e gli esempi corrispondenti nella scheda Intenti. Per vedere i nuovi intenti potresti dover aggiornare la pagina.

## Esportazione di intenti
{: #export_intents}

Puoi esportare una serie di intenti in un file CSV in modo da poterli importare e riutilizzare in un'altra applicazione di conversazione.

1.  Nella scheda Intenti, seleziona ![Icona Esporta](images/ExportIcon.png)

    ![Opzioni Esporta ed Elimina](images/ExportIntent1.png)

1.  Seleziona gli intenti desiderati e fai clic sul pulsante **Esporta**.
    ![Selezione di entità per i pulsanti Esporta ed Elimina](images/ExportIntent2.png)

## Eliminazione di intenti
{: #delete_intents}

Puoi selezionare una serie di intenti da eliminare.

**IMPORTANTE**: eliminando gli intenti elimini anche tutti gli esempi associati e questi elementi non potranno più essere recuperati. Tutti i nodi di dialogo che fanno riferimento a questi intenti devono essere aggiornati manualmente per non fare più riferimento al contenuto eliminato.

1.  Nella scheda Intenti, seleziona ![Icona Elimina](images/DeleteIcon.png)

    ![Opzioni Esporta ed Elimina](images/ExportIntent3.png)

1.  Seleziona gli intenti che vuoi eliminare e fai clic sul pulsante **Elimina**. **Nota**: la funzione di eliminazione supporta l'eliminazione in massa di intenti.

## Test degli intenti
{: #testing-your-intents}

Dopo aver finito di creare i nuovi intenti, puoi testare il sistema per vedere se riconosce i tuoi intenti nel modo previsto. 

1.  Nello strumento {{site.data.keyword.conversationshort}}, seleziona l'icona ![Chiedi a Watson](images/ask_watson.png).

1.  Nel riquadro Provalo, immetti una domanda o un'altra stringa di testo e premi Invio per vedere quale intento viene riconosciuto. Se viene riconosciuto l'intento errato, puoi migliorare il tuo modello aggiungendo questo testo come esempio all'intento corretto.

    Se di recente hai apportato delle modifiche nel tuo spazio di lavoro, potresti visualizzare un messaggio che indica che il sistema è ancora in fase di addestramento. Se visualizzi questo messaggio, attendi che l'addestramento venga completato prima di eseguire il test:
    {: tip}

    ![Schermata che mostra il messaggio della fase di addestramento](images/training.png)

    La risposta indica quale intento è stato riconosciuto dal tuo input.

    ![Schermata del test degli intenti](images/test_intents.png)

1.  Se il sistema non ha riconosciuto l'intento corretto, puoi correggerlo. Per correggere l'intento riconosciuto, seleziona l'intento visualizzato e quindi seleziona l'intento corretto dall'elenco. Dopo aver inviato la tua correzione, il sistema si riaddestra automaticamente per incorporare i nuovi dati.

    ![Schermata della correzione di un intento riconosciuto](images/correct_intent.png)

1.  Se l'input non è correlato alla tua applicazione, puoi indicarlo. Seleziona l'intento visualizzato e scegli **Contrassegna come irrilevante**.

    ![Schermata di Contrassegna come irrilevante](images/irrelevant.png)

Se i tuoi intenti non vengono riconosciuti correttamente, puoi apportare i seguenti tipi di modifiche:

- Aggiungere il testo non riconosciuto come esempio all'intento corretto.
- Spostare gli esempi esistenti da un intento a un altro.
- Valutare se i tuoi intenti sono troppo simili e ridefinirli come appropriato.

## Punteggio assoluto e Contrassegna come irrilevante

A partire da febbraio 2017, è disponibile un nuovo algoritmo per calcolare il punteggio di affidabilità dell'intento e restituire gli intenti. Puoi anche contrassegnare gli input come "irrilevanti". Per queste modifiche, potrebbe essere necessario [aggiornare il tuo spazio di lavoro![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](upgrading.html){: new_window}.

### Punteggio assoluto

Adesso calcoliamo il punteggio di affidabilità di ogni singolo intento, non in relazione ad altri intenti. Ciò consente la flessibilità di avere più intenti restituiti. Significa anche che il sistema potrebbe non restituire alcun intento. Se il sistema ha una bassa confidenza (inferiore a 0,2) che un intento si riferisca all'input utente, non restituirà un intento.

Poiché i punteggi di affidabilità dell'intento cambiano, i tuoi dialoghi potrebbero richiedere una ristrutturazione. Ad esempio, se hai condizionato il tuo dialogo con un intento che ora ha una bassa confidenza, la risposta del sistema non sarà più corretta.

### Contrassegna come irrilevante
{: #mark-irrelevant}

Per la disponibilità di questa funzione, pupi fare riferimento a [Lingue supportate](lang-support.html).

Dopo aver aggiornato il tuo spazio di lavoro, puoi [testare l'input](#testing-your-intents) nel pannello Provalo per vedere le modifiche. Puoi utilizzare "Contrassegna come irrilevante" per indicare che l'input non è correlato alla tua applicazione.

Se hai un intento, ad esempio #off_topic, per quegli input che sono fuori ambito o fuori tema, elimina l'intento e testa il tuo spazio di lavoro contrassegnando gli input come irrilevanti.

> **Importante**: gli input contrassegnati come irrilevanti vengono memorizzati nello spazio di lavoro e inclusi come parte dei dati di addestramento. Assicurati di voler apportare questa modifica.
> - Non è possibile accedere o modificare successivamente gli input negli strumenti.
> - L'unico modo per rimuovere la tag "Irrilevante" è utilizzare lo stesso input nel pannello Provalo e quindi modificare l'intento.
