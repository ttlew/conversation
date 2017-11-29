---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-24"

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

# Creazione di un'applicazione client

Hai già un dialogo di lavoro. Adesso, vuoi sviluppare l'applicazione che interagirà con i tuoi utenti e comunicherà con il servizio {{site.data.keyword.conversationfull}}.
{: shortdesc}

Gli esempi di codice in questa esercitazione vengono scritti in JavaScript utilizzando l'SDK Watson Node.js, ma puoi utilizzare altri linguaggi. Installa l'SDK Watson [ ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} per il tuo linguaggio di programmazione e consulta la documentazione dell'SDK per ulteriori informazioni.
{: tip}

## Configurazione del servizio di conversazione

L'applicazione di esempio che verrà creata in questa sezione implementa diverse funzioni di un assistente personale cognitivo. Il codice applicativo verrà collegato a uno spazio di lavoro {{site.data.keyword.conversationshort}}, in cui avviene l'elaborazione cognitiva (come il rilevamento degli intenti dell'utente).

Prima di continuare con questo esempio, devi configurare lo spazio di lavoro {{site.data.keyword.conversationshort}} richiesto:

1.  Scarica il <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">file JSON</a> dello spazio di lavoro.
1.  [Importa lo spazio di lavoro](configure-workspace.html#creating-workspaces) in un'istanza del servizio {{site.data.keyword.conversationshort}}.

## Recupero delle informazioni sul servizio

Per accedere alle API REST del servizio {{site.data.keyword.conversationshort}}, la tua applicazione deve essere in grado di autenticarsi con {{site.data.keyword.Bluemix}} e connettersi allo spazio di lavoro {{site.data.keyword.conversationshort}} appropriato. Dovrai copiare le credenziali del servizio e l'ID spazio di lavoro e incollarli nel tuo codice applicativo.

Per accedere alle credenziali del servizio e all'ID spazio di lavoro dal tuo spazio di lavoro, seleziona il menu ![Menu](images/Menu_16.png), scegli **Distribuisci** e vai quindi alla scheda **Credenziali**.

Puoi accedere alle credenziali del servizio anche dal tuo dashboard Bluemix.

## Comunicazione con il servizio di conversazione

L'interazione con il servizio {{site.data.keyword.conversationshort}} è semplice. Diamo un'occhiata a un esempio di Node.js che si connette al servizio, invia un singolo messaggio e registra l'output nella console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}

Il primo passo è quello di creare un wrapper per il servizio {{site.data.keyword.conversationshort}}.

Il wrapper è un oggetto che utilizzerai per inviare e ricevere l'input dal servizio. Quando crei il wrapper del servizio, specifica le credenziali di autenticazione dalla chiave del servizio, oltre alla versione dell'API {{site.data.keyword.conversationshort}} che stai utilizzando.

In questo esempio di Node.js, il wrapper è un'istanza di `ConversationV1`, memorizzato nella variabile `conversation`. Gli SDK Watson per gli altri linguaggi forniscono meccanismi equivalenti per creare un'istanza di un wrapper del servizio.

Dopo aver creato il wrapper del servizio, lo utilizziamo per inviare un messaggio al servizio {{site.data.keyword.conversationshort}}. In questo esempio, il messaggio è vuoto; vogliamo solo attivare il nodo conversation_start e pertanto non ci serve alcun testo di input.

Utilizza il comando `node <filename.js>` per eseguire l'applicazione di esempio.

**Nota:** assicurati di aver installato l'SDK Watson Node.js utilizzando `npm install watson-developer-cloud`.

Supponendo che tutto funzioni come previsto, il servizio {{site.data.keyword.conversationshort}} restituisce l'output dal dialogo, che viene quindi registrato nella console.

```
Welcome to the Conversation example!
```
{: screen}

Questo output ci indica che abbiamo comunicato correttamente con il servizio {{site.data.keyword.conversationshort}} e abbiamo ricevuto il messaggio di benvenuto specificato dal nodo conversation_start nel dialogo. Ora possiamo aggiungere un'interfaccia utente, che consente di elaborare l'input utente.

## Elaborazione dell'input utente per rilevare gli intenti

Per poter elaborare l'input utente, dobbiamo aggiungere un'interfaccia utente alla nostra applicazione. Per questo esempio, manterremo le cose semplici e utilizzeremo l'input e l'output standard. Per farlo, possiamo utilizzare il modulo prompt-sync di Node.js. (Puoi installare prompt-sync utilizzando `npm install prompt-sync`.)

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}

Questa versione dell'applicazione inizia allo stesso modo di prima: inviando un messaggio vuoto al servizio {{site.data.keyword.conversationshort}} per avviare la conversazione.

La funzione `processResponse()` adesso visualizza qualsiasi intento rilevato dal dialogo insieme al testo di output e quindi richiede il prossimo ciclo di input utente.

Ma qualcosa ancora non va bene:

```
Welcome to the Conversation example!
>> hello
Detected intent: #hello
Welcome to the Conversation example!
>> what time is it?
Detected intent: #time
Welcome to the Conversation example!
>> goodbye
Detected intent: #goodbye
Welcome to the Conversation example!
>>
```
{: screen}

Il servizio {{site.data.keyword.conversationshort}} sta rilevando gli intenti corretti, eppure ogni turno della conversazione restituisce il messaggio di benvenuto dal nodo conversation_start (`Welcome to the Conversation example!`).

Questo accade perché il servizio {{site.data.keyword.conversationshort}} è senza stato; è responsabilità dell'applicazione mantenere le informazioni sullo stato. Poiché non stiamo ancora facendo nulla per mantenere lo stato, il servizio {{site.data.keyword.conversationshort}} vede ogni ciclo di input utente come il primo turno di una nuova conversazione, attivando il nodo conversation_start.

## Mantenimento dello stato

Le informazioni sullo stato per la tua conversazione vengono mantenute utilizzando il *contesto*. Il contesto è un oggetto JSON che viene trasmesso avanti e indietro tra la tua applicazione e il servizio {{site.data.keyword.conversationshort}}. È responsabilità della tua applicazione mantenere il contesto da un turno della conversazione a quello successivo.

Il contesto include un identificativo univoco per ogni conversazione con un utente, nonché un contatore che viene incrementato con ogni turno della conversazione. La nostra versione precedente dell'esempio non manteneva il contesto, il che significa che ogni ciclo di input sembrava essere l'inizio di una nuova conversazione. Possiamo risolvere questo problema salvando il contesto e rinviandolo ogni volta al servizio {{site.data.keyword.conversationshort}}.

Oltre a mantenere il nostro posto nella conversazione, il contesto può essere utilizzato anche per memorizzare qualsiasi altro dato che si desidera passare avanti e indietro tra l'applicazione e il servizio {{site.data.keyword.conversationshort}}. Questi possono includere i dati persistenti che vuoi mantenere durante la conversazione (come il nome o il numero di account del cliente) o qualsiasi altro dato da monitorare (ad esempio, lo stato corrente delle impostazioni di opzioni).

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
    var newMessageFromUser = prompt('>> ');
    // Send back the context to maintain state.
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}

L'unica modifica rispetto all'esempio precedente è che, con ogni ciclo della conversazione, adesso rimandiamo l'oggetto `response.context` che abbiamo ricevuto nel ciclo precedente:

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}

Ciò assicura che il contesto venga mantenuto da un turno all'altro e quindi il servizio {{site.data.keyword.conversationshort}} non pensa più che ogni turno sia il primo:

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

Stiamo facendo progressi! Il servizio {{site.data.keyword.conversationshort}} sta riconoscendo correttamente i nostri intenti e il dialogo restituisce il testo di output corretto (dove fornito) per ogni intento.

Tuttavia, non succede altro. Quando chiediamo l'ora non otteniamo risposta e quando diciamo goodbye, la conversazione non finisce. Ciò è dovuto al fatto che questi intenti richiedono ulteriori azioni da parte dell'applicazione.

## Implementazione delle azioni dell'applicazione

Oltre a visualizzare il testo di output per l'utente, il nostro dialogo utilizza l'oggetto `output` nel JSON di risposta per segnalare quando l'applicazione deve eseguire un'azione, in base agli intenti rilevati.

Questi indicatori di azione vengono inviati utilizzando la proprietà `action`, che il nostro dialogo definisce come parte del JSON di risposta. Quando il dialogo determina che l'applicazione deve fare qualcosa, imposta il valore di `action` sul valore appropriato, `display_time` o `end_conversation`.

Tieni presente che `output` è solo un oggetto JSON e puoi aggiungere qualsiasi contenuto valido desideri. Per un'applicazione più complessa, potresti utilizzare un array con più indicatori di azione.

Ma nel nostro esempio, utilizziamo una semplice coppia chiave/valore che supporta un singolo indicatore di azione. Il codice applicativo deve controllare il valore della proprietà `action` nella risposta e quindi eseguire l'azione specificata. (Questa versione rimuove anche la visualizzazione degli intenti rilevati, ora che siamo sicuri che questi sono stati identificati correttamente.)

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // User said goodbye, so we're done.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Display the output from dialog, if any.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      input: { text: newMessageFromUser },
      // Send back the context to maintain state.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}

Adesso la funzione processResponse() controlla il valore della proprietà `action` dell'oggetto `output` ricevuto dal servizio {{site.data.keyword.conversationshort}}. Se il valore è `display_time` o `end_conversation`, l'applicazione esegue l'azione appropriata. 

```
Welcome to the Conversation example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

Ci siamo riusciti! L'applicazione adesso utilizza il servizio {{site.data.keyword.conversationshort}} per identificare gli intenti nell'input in linguaggio naturale, visualizza le risposte appropriate e implementa le azioni richieste. 

Naturalmente, un'applicazione del mondo reale può utilizzare un'interfaccia utente più sofisticata, ad esempio una finestra di chat web. E può implementare azioni più complesse, possibilmente integrando un database clienti o altri sistemi aziendali. Ma i principi di base su come l'applicazione interagisce con il servizio {{site.data.keyword.conversationshort}} restano uguali.

Per alcuni esempi più complessi, dai un'occhiata alle Applicazioni di esempio nel riquadro di navigazione.
