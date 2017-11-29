---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-15"

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

# Accesso e valutazione degli oggetti

Le espressioni valide nelle condizioni sono scritte in linguaggio SpEL (Spring Expression). Per ulteriori informazioni, vedi [Linguaggio Spring Expression Language (SpEL)![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Variabili globali integrate

Sono disponibili le seguenti variabili globali:

| Variabile globale     | Definizione |
|----------------------|------------|
| *anything_else*      | L'ultimo nodo dell'intero dialogo. Quando l'input utente non può essere associato a un intento, viene eseguito questo nodo.|
| *context*            | Parte di oggetto JSON del messaggio di conversazione elaborato.|
| *conversation_start* | Un valore booleano che è true nel primo turno di conversazione del dialogo (può essere utilizzato in una condizione di un nodo di dialogo per definire un messaggio di benvenuto). |
| *entities[ ]*        | Elenco di entità che supporta l'accesso predefinito al primo elemento. |
| *input*              | Parte di oggetto JSON del messaggio di conversazione elaborato.|
| *intents[ ]*         | Elenco di intenti che supporta l'accesso predefinito al primo elemento. |
| *output*             | Parte di oggetto JSON del messaggio di conversazione elaborato.|

## Accesso alle entità

L'array di entità contiene una o più entità. 

Mentre testi il tuo dialogo, puoi vedere i dettagli delle entità riconosciute nell'input utente specificando questa espressione in una risposta del nodo di dialogo:

```json
<? entities ?>
```
{: codeblock}

Per l'input utente, *Hello now*, il servizio riconosce le entità di sistema @sys-date e @sys-time, pertanto la risposta contiene questi oggetti di entità:

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Sintassi abbreviata per le entità

La seguente tabella mostra esempi di sintassi abbreviata che puoi utilizzare per fare riferimento alle entità.

| Sintassi abbreviata    | Sintassi completa in SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

In SpEL, il punto interrogativo `(?)` impedisce di attivare un'eccezione di puntatore vuoto quando un oggetto di entità è null.

Se il valore di entità che vuoi controllare contiene un carattere `)`, non puoi utilizzare l'operatore `:` per il confronto.  Ad esempio, se vuoi controllare se l'entità città è `Dublin (Ohio)`, devi utilizzare `@city == 'Dublin (Ohio)'` invece di `@city:(Dublin (Ohio))`.

### Importanza del posizionamento delle entità nell'input

Utilizza l'espressione completa SpEL se il posizionamento delle entità nell'input è importante. La condizione `entities['city']?.contains('Boston')` restituisce true quando almeno un'entità città 'Boston' viene trovata in tutte le entità @city, indipendentemente dal posizionamento.

Ad esempio, un utente invia `"Voglio andare da Toronto a Boston."` Le entità `@city:Toronto` e `@city:Boston` vengono rilevate e sono rappresentate in queste entità:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

La condizione `@city.contains('Boston')` in un nodo di dialogo restituisce true anche se Boston è la seconda entità rilevata.

### Proprietà di entità

Ogni entità ha una serie di proprietà associate. Puoi accedere alle informazioni su un'entità attraverso le sue proprietà.

| Proprietà              | Definizione | Suggerimenti sull'utilizzo |
|-----------------------|------------|------------|
| *confidence*          | Una percentuale decimale che rappresenta l'affidabilità del servizio nell'entità riconosciuta. L'affidabilità di un'entità è 0 o 1, a meno che non sia stata attivata la corrispondenza fuzzy delle entità. Se la corrispondenza fuzzy è abilitata, la soglia del livello di affidabilità predefinita è 0,3. A prescindere dell'abilitazione della corrispondenza fuzzy, le entità di sistema hanno sempre un livello di affidabilità pari a 1. | Puoi utilizzare questa proprietà in una condizione in modo che restituisca false se il livello di affidabilità non è superiore a una percentuale da te specificata. |
| *location*            | Un offset di caratteri in base zero che indica dove iniziano e terminano i valori di entità rilevati nel testo di input. | Utilizza `.literal` per estrarre la parte di testo tra i valori di indice iniziali e finali memorizzati nella proprietà di posizione.|
| *value*               | Il valore di entità identificato nell'input. | Questa proprietà restituisce il valore di entità come definito nei dati di addestramento, anche se la corrispondenza è avvenuta in uno dei suoi sinonimi associati. Puoi utilizzare `.values` per acquisire più ricorrenze di un'entità che potrebbero essere presenti nell'input utente. |

### Esempi di utilizzo delle proprietà di entità
Nei seguenti esempi, lo spazio di lavoro contiene un'entità aeroporto che include il valore JFK e il sinonimo 'Aeroporto Kennedy". L'input utente è *Voglio andare all'aeroporto Kennedy*.

- Per restituire una risposta specifica se l'entità 'JFK' viene riconosciuta nell'input utente, puoi aggiungere questa espressione alla condizione di risposta:
  `entities.airport[0].value == 'JFK'`
  o
  `@airport = "JFK"`
- Per restituire il nome entità come specificato dall'utente nella risposta di dialogo, utilizza la proprietà .literal:
  `So you want to go to <?entities.airport[0].literal?>...`
  o
  `So you want to go to @airport.literal ...`

  Entrambi i formati restituiscono 'So you want to go to Kennedy Airport...' nella risposta.

- Espressioni come `@airport:(JFK)` o `@airport.contains('JFK')` fanno sempre riferimento al **valore** dell'entità (in questo esempio, `JFK`).
- Per essere più restrittivi sui termini che vengono identificati come aeroporti nell'input quando è abilitata la corrispondenza fuzzy, puoi specificare questa espressione in una condizione del nodo, ad esempio: `@airport && @airport.confidence > 0.7`. Il nodo verrà eseguito solo se il servizio è sicuro al 70% che il testo di input contiene un riferimento all'aeroporto.

In questo esempio, l'input utente è *Ci sono dei posti per cambiare la valuta in JFK, Logan e O'Hare?*

- Per acquisire più ricorrenze di un tipo di entità nell'input utente, utilizza una sintassi come questa:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Per fare successivamente riferimento all'elenco acquisito in una risposta di dialogo, aggiungi questa sintassi:
  `Hai chiesto informazioni su questi aeroporti: <? $airports.join(', ') ?>.`
  Viene visualizzato come questo:
  `Hai chiesto informazioni su questi aeroporti: JFK, Logan, O'Hare.`

## Accesso agli intenti

L'array di intenti contiene uno o più intenti riconosciuti nell'input utente, ordinati in ordine decrescente di affidabilità. Ogni intento ha una sola proprietà: la proprietà di affidabilità. La proprietà di affidabilità è una percentuale decimale che rappresenta l'affidabilità del servizio nell'intento riconosciuto.

Mentre testi il tuo dialogo, puoi vedere i dettagli degli intenti riconosciuti nell'input utente specificando questa espressione in una risposta del nodo di dialogo:

```json
<? intents ?>
```
{: codeblock}

Per l'input utente, *Hello now*, il servizio è sicuro che l'intento #greeting sia la corrispondenza migliore per l'input utente e quindi elenca per primi i dettagli dell'oggetto intento #greeting. La risposta include anche tutti gli altri intenti definiti nello spazio di lavoro, anche se la loro affidabilità è così bassa che arrotonda a 0.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

I seguenti esempi mostrano come controllare un valore di intento:

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` è diverso da `intents[0] == 'help'` perché `intent == 'help'` non genera un'eccezione se non viene rilevato alcun intento. Viene valutato come true solo se l'affidabilità dell'intento supera una soglia.  Se lo desideri, puoi specificare un livello di affidabilità personalizzato per una condizione, ad esempio `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

### Sintassi abbreviata per gli intenti

La seguente tabella mostra esempi di sintassi abbreviata che puoi utilizzare per fare riferimento agli intenti.

| Sintassi abbreviata    | Sintassi completa in SpEL                      |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` o `#i_am_lost` | <code>(intent == 'help' &#124;&#124; intent == 'I_am_lost')</code> |

## Accesso all'input

L'oggetto JSON di input contiene una sola proprietà: la proprietà di testo. La proprietà di testo rappresenta il testo dell'input utente.

### Esempi di utilizzo delle proprietà di input

Il seguente esempio mostra come accedere all'input:

- Per eseguire un nodo se l'input utente è "Yes", aggiungi questa espressione alla condizione del nodo:
  `input.text == 'Yes'`

Puoi utilizzare uno qualsiasi dei [Metodi stringa](/docs/services/conversation/dialog-methods.html#strings) per valutare o manipolare il testo dell'input utente. Ad esempio:

- Per controllare se l'input utente contiene "Yes", utilizza: `input.text.contains( 'Yes' )`.
- Restituisce true se l'input utente è un numero: `input.text.matches( '[0-9]+' )`.
- Per controllare se la stringa di input contiene dieci caratteri, utilizza: `input.text.length() == 10`.

## Sintassi abbreviata per le variabili di contesto

La seguente tabella mostra esempi di sintassi abbreviata che puoi utilizzare per scrivere variabili di contesto nelle espressioni di condizione.

| Sintassi abbreviata    | Sintassi completa in SpEL                      |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

Nei nomi delle variabili di contesto puoi includere caratteri speciali come trattini o punti. Tuttavia, ciò può causare problemi quando viene valutata l'espressione SpEL. Ad esempio, il trattino potrebbe essere interpretato come un segno meno. Per evitare tali problemi, fai riferimento alla variabile utilizzando la sintassi completa dell'espressione o la sintassi abbreviata `$(variable-name)` e non utilizzare i seguenti caratteri speciali nel nome:

- Parentesi ()
- Più di un apostrofo ''
- Virgolette "

## Valutazione

Per espandere i valori delle variabili all'interno di altre variabili o applicare metodi alle variabili, utilizza la sintassi dell'espressione `<? expression ?>`. Ad esempio:

- **Espansione di una proprietà**
    - `"output":{"text":"Your name is <? context.userName ?>"}`
    oppure
    - `"output":{"text":"Your name is $userName"}` nella sintassi abbreviata
- **Incremento di una proprietà numerica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Richiamo di metodi, come l'aggiunta, su proprietà e oggetti globali**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

### Formato oggetto o stringa JSON

Quando utilizzi la sintassi SpEL completa nella risposta di dialogo, racchiudi l'espressione tra `<?` e `?>` per renderla in formato stringa. Quando utilizzi la sintassi SpEL completa in una condizione, non includere la sintassi `<? ?>` circostante.

Se specifichi un'espressione SpEL in una condizione, le informazioni vengono restituite in formato oggetto in modo che il valore risultante possa conservare il proprio tipo di dati ed essere utilizzato in equazioni o altre espressioni. Se specifichi più di un'espressione SpEL in una condizione o includi l'espressione come parte di una stringa, le informazioni verranno restituite in formato stringa.  

Ad esempio, puoi aggiungere questa espressione a una risposta del nodo di dialogo per restituire le entità riconosciute nell'input utente:

```json
The entities are <? entities ?>.
```
{: codeblock}

Se l'utente specifica *Hello now* come input, le informazioni dell'entità vengono fornite in formato stringa.

```json
The entities are 2017-08-07, 15:09:49.
```
{: codeblock}

Puoi aggiungere questa espressione a una risposta del nodo di dialogo per restituire gli intenti riconosciuti nell'input utente:

```json
The intents are <? intents ?>.
```
{: codeblock}

Se l'utente specifica *Hello now* come input, le informazioni dell'intento vengono fornite in formato stringa.

```json
The intents are [
{"intent":"greeting","confidence":0.9331061244010925},
{"intent":"yes","confidence":0.06050306558609009},
{"intent":"pizza-order","confidence":0.052069634199142456},
...
]
```
{: codeblock}
