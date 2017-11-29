---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-10"

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

# Esercitazione introduttiva
{: #gettingstarted}

In questa breve esercitazione, viene presentato lo strumento {{site.data.keyword.conversationshort}} e viene descritto il processo per creare la tua prima conversazione.
{: shortdesc}

## Prima di iniziare
{: #prerequisites}

Se hai già creato un'istanza del servizio, hai già tutti i prerequisiti. Vai al passo 1.
{: tip}

1.  Vai al servizio [{{site.data.keyword.conversationshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/catalog/services/conversation/){: new_window} e registrati per un account {{site.data.keyword.Bluemix_notm}} gratuito o esegui l'accesso.
1.  Dopo aver eseguito l'accesso, immetti `conversation-tutorial` nel campo **Nome servizio** della pagina {{site.data.keyword.conversationshort}} e fai clic su **Crea**.

## Passo 1: avvia lo strumento
{: #launch-tool}

Dopo aver creato l'istanza del servizio, verrai portato sul dashboard dell'istanza. Da lì, avvia lo strumento {{site.data.keyword.conversationshort}}.

Fai clic su **Gestisci** e quindi su **Avvia strumento**.

![Mostra la pagina Gestisci di IBM Bluemix Watson in cui avvii lo strumento](images/gs-launch-tool.png)

Ti potrebbe essere richiesto di accedere allo strumento separatamente. In tal caso, fornisci le tue credenziali IBM Bluemix per eseguire l'accesso.

## Passo 2: crea uno spazio di lavoro
{: #create-workspace}

Il primo passo da compiere nello strumento {{site.data.keyword.conversationshort}} è creare uno spazio di lavoro.

Uno [*spazio di lavoro*](configure-workspace.html) è un contenitore delle risorse che definiscono il flusso di conversazione. 

1.  Nello strumento {{site.data.keyword.conversationshort}}, fai clic su **Crea**.
1.  Immetti il nome `Esercitazione di conversazione` per il tuo spazio di lavoro e fai clic su **Crea**. Verrai portato alla scheda **Intenti** del tuo nuovo spazio di lavoro.

![Mostra un'animazione di un utente che crea uno spazio di lavoro Esercitazione di conversazione.](images/gs-create-workspace-animated.gif)

## Passo 3: crea intenti
{: #create-intents}

Un [intento](intents.html) rappresenta lo scopo dell'input di un utente. Gli intenti possono essere considerati come le azioni che gli utenti potrebbero voler eseguire con la tua applicazione.

Per questo esempio, vogliamo mantenere le cose semplici e definire solo due intenti: uno per dire hello e uno per dire goodbye.

1.  Assicurati di trovarti nella scheda Intenti. (Dovresti essere già lì se hai appena creato lo spazio di lavoro.)
1.  Fai clic su **Crea nuovo**.
1.  Denomina l'intento `hello`.
1.  Immetti `hello` come **Esempio utente** e premi Invio.

   Gli *Esempi* indicano al servizio {{site.data.keyword.conversationshort}} quali tipi di input utente vuoi far corrispondere all'intento. Più esempi fornisci, più accurato sarà il servizio nel riconoscere gli intenti dell'utente.
1.  Aggiungi altri quattro esempi e fai clic su **Fatto** per terminare la creazione dell'intento #hello:
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

   ![Mostra un'animazione di un utente che crea un intento #hello con espressioni di esempio.](images/gs-add-intents-animated.gif)

1.  Crea un altro intento denominato #goodbye con questi cinque esempi:
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

Hai creato due intenti, #hello e #goodbye, e hai fornito l'input utente di esempio per addestrare {{site.data.keyword.watson}} a riconoscere questi intenti nell'input degli utenti.

![Mostra la pagina Intenti che elenca gli intenti #goodbye e #hello](images/gs-add-intents-result.png)

## Passo 4: crea un dialogo
{: #build-dialog}

Un [dialogo](dialog-build.html) definisce il flusso della tua conversazione sotto forma di una struttura ad albero logica. Ogni nodo della struttura ad albero ha una condizione che lo attiva, in base all'input utente.

Creiamo un semplice dialogo che gestisce gli intenti #hello e #goodbye, ognuno con un singolo nodo.

### Aggiunta di un nodo iniziale

1.  Nello strumento {{site.data.keyword.conversationshort}}, fai clic sulla scheda **Dialogo**.
1.  Fai clic su **Crea**. Vedrai due nodi:
    - **Benvenuto**: contiene un messaggio iniziale che viene visualizzato agli utenti la prima volta che si collegano al bot.
    - **Altro**: contiene frasi che vengono utilizzate per rispondere agli utenti quando il loro input non viene riconosciuto. 

    ![Mostra la struttura ad albero di dialogo con i nodi Benvenuto e Altro](images/gs-add-dialog-node-animated-cover.png)
1.  Fai clic sul nodo **Benvenuto** per aprirlo nella vista di modifica.
1.  Sostituisci la risposta predefinita con il testo `Benvenuto all'esercitazione di conversazione!`.

    ![Mostra il nodo Benvenuto aperto nella vista di modifica](images/gs-edit-welcome-node.png)
1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

Hai creato un nodo di dialogo che viene attivato dalla condizione `welcome`, che è una condizione speciale che indica che l'utente ha iniziato una nuova conversazione. Il tuo nodo specifica che quando inizia una nuova conversazione, il sistema deve rispondere con il messaggio di benvenuto.

### Test del nodo iniziale

Puoi testare il tuo dialogo in qualsiasi momento per verificare come funziona. Testiamolo adesso.

- Fai clic sull'icona ![Chiedi a Watson](images/ask_watson.png) per aprire il riquadro "Provalo". Dovresti vedere il tuo messaggio di benvenuto.

    ![Test del nodo di dialogo](images/gs-tryitout-welcome-node.png)

### Aggiunta di nodi per gestire gli intenti

Adesso aggiungiamo i nodi per gestire i nostri intenti tra il nodo `Benvenuto` e il nodo `Altro`.

1.  Fai clic sull'icona Altro ![Altre opzioni](images/kabob.png) sul nodo **Benvenuto** e seleziona quindi **Aggiungi nodo in basso**.
1.  Immetti `#hello` nel campo **Immetti una condizione** di questo nodo. Quindi, seleziona l'opzione **#hello**.
1.  Aggiungi la risposta `Good day to you.`
1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

   ![Mostra un'animazione di un utente che aggiunge un nodo hello al dialogo.](images/gs-add-dialog-node-animated.gif)
1.  Fai clic sull'icona Altro ![Altre opzioni](images/kabob.png) su questo nodo e seleziona quindi **Aggiungi nodo in basso** per creare un nodo peer. Nel nodo peer, specifica `#goodbye` come condizione e `OK. See you later!` come risposta.

    ![Aggiunta di nodi per gli intenti](images/gs-add-dialog-nodes-result.png)

### Test del riconoscimento degli intenti

Hai creato un semplice dialogo per riconoscere e rispondere agli input hello e goodbye. Vediamo come funziona.

1.  Fai clic sull'icona ![Chiedi a Watson](images/ask_watson.png) per aprire il riquadro "Provalo". Viene visualizzato il messaggio di benvenuto.
1.  Nella parte inferiore del riquadro, immetti `Hello` e premi Invio. L'output indica che l'intento #hello è stato riconosciuto e viene visualizzata la risposta appropriata (`Good day to you.`).
1.  Prova il seguente input:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

   ![Mostra un'animazione dell'utente che testa il dialogo nel pannello Provalo](images/gs-test-dialog-animated.gif)

{{site.data.keyword.watson}} può riconoscere i tuoi intenti anche quando il tuo input non corrisponde esattamente agli esempi che hai incluso. Il dialogo utilizza gli intenti per identificare lo scopo dell'input utente indipendentemente dall'esatta formulazione utilizzata e quindi risponde nel modo che hai specificato. 

### Risultato della creazione di un dialogo

Questo è tutto. Hai creato una semplice conversazione con due intenti e un dialogo per riconoscerli.

## Passo 5: riesamina lo spazio di lavoro di esempio
{: #review-sample-workspace}

Apri lo spazio di lavoro di esempio per visualizzare intenti simili a quelli appena creati e molti altri e per vedere come vengono utilizzati in un dialogo complesso.

1.  Torna alla pagina Spazi di lavoro.
   Puoi fare clic sul pulsante ![Pulsante Torna a spazi di lavoro](images/workspaces-button.png) dal menu di navigazione.
1.  Nel tile dello spazio di lavoro **Dashboard automobile - Esempio**, fai clic sul pulsante **Modifica esempio**.

    ![Mostra il tile di esempio Dashboard automobile nella pagina Spazi di lavoro](images/gs-workspace-car-sample.png)

## Operazioni successive
{: #next-steps}

Questa esercitazione si basa su un esempio semplice. Per un'applicazione reale, dovrai definire intenti più interessanti, alcune entità e un dialogo più complesso.

- Prova l'[esercitazione](tutorial.html) avanzata per aggiungere entità e chiarire lo scopo dell'utente.
- [Distribuisci](deploy.html) il tuo spazio di lavoro connettendolo a un'interfaccia utente di front-end, a un social media o a un canale di messaggistica.
- Controlla le [applicazioni di esempio](sample-applications.html).
