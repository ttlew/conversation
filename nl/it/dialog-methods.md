---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-08"

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

# Metodi per elaborare i valori

Utilizza questi metodi per elaborare i valori estratti dalle espressioni dell'utente a cui vuoi fare riferimento in una variabile di contesto, in una condizione o altrove nella risposta.
{: shortdesc}

>**Nota:** per i metodi che prevedono le espressioni regolari, vedi [Riferimento alla sintassi RE2](https://github.com/google/re2/wiki/Syntax) per i dettagli sulla sintassi da utilizzare quando specifichi l'espressione regolare.

## Array
{: #arrays}

Non puoi utilizzare questi metodi per controllare i valori di un array in una condizione di nodo o una condizione di risposta all'interno dello stesso nodo in cui imposti i valori di array.

### JSONArray.append(object)

Questo metodo aggiunge un nuovo valore al JSONArray e restituisce il JSONArray modificato.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effettua questo aggiornamento:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: screen}

### JSONArray.contains(object value)

Questo metodo restituisce true se il JSONArray di input contiene il valore di input.

Per questo contesto di runtime di dialogo che viene impostato da un nodo precedente o da un'applicazione esterna:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Nodo di dialogo o condizione di risposta:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Risultato: `True` perché l'array contiene l'elemento ham.

### JSONArray.get(integer)

Questo metodo restituisce l'indice di input dal JSONArray.

Per questo contesto di runtime di dialogo che viene impostato da un nodo precedente o da un'applicazione esterna:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Nodo di dialogo o condizione di risposta:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Risultato:
`True` perché l'array nidificato contiene `one` come valore.

Risposta:

```json
"output": {
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

Questo metodo restituisce un elemento casuale dal JSONArray di input.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Risultato: `"ham is a great choice!"` o `"onion is a great choice!"` o `"olives is a great choice!"`

**Nota:** il testo di output risultante viene scelto in modo casuale.

### JSONArray.join(string delimiter)

Questo metodo unisce tutti i valori di questo array a una stringa. I valori vengono convertiti in stringa e delimitati dal delimitatore di input.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Risultato:

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(integer)

Questo metodo rimuove l'elemento nella posizione dell'indice dal JSONArray e restituisce il JSONArray aggiornato.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effettua questo aggiornamento:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.removeValue(object)

Questo metodo rimuove la prima ricorrenza del valore dal JSONArray e restituisce il JSONArray aggiornato.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effettua questo aggiornamento:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.set(integer index, object value)

Questo metodo imposta l'indice di input del JSONArray sul valore di input e restituisce il JSONArray modificato.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

Questo metodo restituisce la dimensione del JSONArray come numero intero.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effettua questo aggiornamento:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(String regexp)

Questo metodo suddivide la stringa di input utilizzando l'espressione regolare di input. Il risultato è un JSONArray di stringhe.

Per questo input:

```
"bananas;apples;pears"
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## Data e ora
{: #date-time}

Sono disponibili diversi metodi per lavorare con la data e ora.

### .after(String date/time)

- Determina se il valore data/ora è dopo l'argomento data/ora.
- Simile a `.before()`.

### .before(String date or time)

- Ad esempio:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Determina se il valore data/ora è prima dell'argomento data/ora.
- Se si confrontano elementi diversi, come `time vs. date`, `date vs. time` e `time vs. date and time`, il metodo restituisce false e viene stampata un'eccezione nel log JSON di risposta `output.log_messages`.
    - Ad esempio, `@sys-date.before(@sys-time)`.
- Se si confronta `date and time vs. time` il metodo ignora la data e confronta solo le ore.

### now()

- Funzione statica.
- Restituisce una stringa con la data e ora corrente in formato `yyyy-MM-dd HH:mm:ss`.
- Gli altri metodi di data/ora possono essere richiamati sui valori data-ora restituiti da questa funzione e possono essere passati come argomento.
- Se la variabile di contesto `$timezone` è impostata, questa funzione restituisce date e ore nel fuso orario del client.

Esempio di un nodo di dialogo con `now()` utilizzato nel campo di output:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Esempio di `now()` nelle condizioni del nodo (per decidere se è ancora mattina):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

Formatta stringhe di data e ora nel formato desiderato per l'output dell'utente.

Restituisce una stringa formattata in base al formato specificato:

- `MM/dd/yyyy` per 12/31/2016
- `h a` per 10pm

Per restituire il giorno della settimana:

- `E` per martedì
- `u` per l'indice di giorni (1 = lunedì, ..., 7 = domenica)

Ad esempio, questa definizione di variabile di contesto crea una variabile $time che salva il valore 17:30:00 come *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Il formato segue le regole Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html).

>Nota: quando si tenta di formattare solo l'ora, la data viene considerata come `1970-01-01`.

### .sameMoment(String date/time)

- Determina se il valore data/ora è uguale all'argomento data/ora.

### .sameOrAfter(String date/time)

- Determina se il valore data/ora è dopo o uguale all'argomento data/ora.
- Simile a `.after()`.

### .sameOrBefore(String date/time)

- Determina se il valore data/ora è prima o uguale all'argomento data/ora.

Per informazioni sulle entità di sistema che estraggono i valori di data e ora, vedi [Entità @sys-date e @sys-time](system-entities.html#sys-datetime).

## Numeri
{: #numbers}

### Random()

Restituisce un numero casuale. Utilizza una delle seguenti opzioni di sintassi:

- Per restituire un valore booleano casuale (true o false), utilizza `<?new Random().nextBoolean()?>`.
- Per restituire un numero doppio casuale compreso tra 0 (incluso) e 1 (escluso), utilizza `<?new Random().nextDouble()?>`
- Per restituire un numero intero casuale compreso tra 0 (incluso) e un numero da te specificato, utilizza `<?new Random().nextInt(n)?>`  dove n è l'inizio dell'intervallo di numeri desiderato  + 1.
  Ad esempio, se vuoi restituire un numero casuale tra 0 e 10, specifica `<?new Random().nextInt(11)?>`.
- Per restituire un numero intero casuale dall'intervallo completo di valori interi (da -2147483648 a 2147483648), utilizza `<?new Random().nextInt()?>`.

Ad esempio, puoi creare un nodo di dialogo che viene attivato dall'intento #random_number. La prima condizione di risposta potrebbe essere simile alla seguente:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

### toDouble()

  Converte l'oggetto o il campo nel tipo di numero Doppio. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

### toInt()

  Converte l'oggetto o il campo nel tipo di numero Intero. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

### toLong()

  Converte l'oggetto o il campo nel tipo di numero Lungo. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

Per informazioni sulle entità di sistema che estraggono i numeri, vedi [Entità @sys-number](system-entities.html#sys-number).

## Oggetti
{: #objects}

### JSONObject.has(string)

Questo metodo restituisce true se il JSONObject complesso ha una proprietà del nome di input.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Risultato: la condizione è true perché l'oggetto utente contiene la proprietà `first_name`.

### JSONObject.remove(string)

Questo metodo rimuove una proprietà del nome dall'input `JSONObject`. Il `JSONElement` che viene restituito da questo metodo è il `JSONElement` che verrà rimosso.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: screen}

## Stringhe
{: #strings}

### String.append(object)

Questo metodo aggiunge un oggetto di input alla stringa sotto forma di stringa e restituisce una stringa modificata.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

Questo metodo restituisce true se la stringa contiene la sottostringa di input.

Input: "Sì, vorrei andare."

Questa sintassi:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.endsWith(string)

Questo metodo restituisce true se la stringa termina con la sottostringa di input.

Per questo input:

```
"What is your name?".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.extract(String regexp, Integer groupIndex)

Questo metodo restituisce una stringa estratta dall'indice di gruppi specificato dell'espressione regolare di input.

Per questo input:

```
"Hello 123456".
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Importante:** per elaborare `\\d` come espressione regolare, devi eseguire l'escape di entrambe le barre rovesciate aggiungendo un altro `\\`: `\\\\d`

Risultato:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

Questo metodo restituisce true se qualsiasi segmento della stringa corrisponde all'espressione regolare di input. Puoi richiamare questo metodo su un elemento JSONArray o JSONObject e questo convertirà l'array o l'oggetto in una stringa prima di effettuare il confronto.

Per questo input:

```
"Hello 123456".
```
{: screen}

Questa sintassi:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Risultato: la condizione è true perché la parte numerica del testo di input corrisponde all'espressione regolare `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Questo metodo restituisce true se la stringa è una stringa vuota, ma non null.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.length()

Questo metodo restituisce la lunghezza dei caratteri della stringa.

Per questo input:

```
"Hello"
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

Questo metodo restituisce true se la stringa corrisponde all'espressione regolare di input. 

Per questo input:

```
"Hello".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Risultato: la condizione è true perché il testo di input corrisponde all'espressione regolare `\^Hello\$`.

### String.startsWith(string)

Questo metodo restituisce true se la stringa inizia con la sottostringa di input.

Per questo input:

```
"What is your name?".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.substring(int beginIndex, int endIndex)

Questo metodo ottiene una sottostringa con il carattere in `beginIndex` e l'ultimo carattere impostato sull'indice prima di `endIndex`.
Il carattere endIndex non è incluso.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

Questo metodo restituisce la stringa originale convertita in lettere minuscole.

Per questo input:

```
"This is A DOG!"
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

Questo metodo restituisce la stringa originale convertita in lettere maiuscole.

Per questo input:

```
"hi there".
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

Questo metodo elimina gli spazi all'inizio e alla fine della stringa e restituisce la stringa modificata.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

Per informazioni sulle entità di sistema che estraggono le stringhe, vedi [Entità di sistema](system-entities.html).
