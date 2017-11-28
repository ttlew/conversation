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

# Auf Objekte zugreifen und Objekte auswerten

Gültige Ausdrücke in Bedingugen werden in SpEL (Spring Expression Language) geschrieben. Weitere Informationen finden Sie auf der Seite zu [Spring Expression Language (SpEL) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Integrierte globale Variablen

Die folgenden globalen Variablen sind verfügbar:

| Globale Variable     | Definition |
|----------------------|------------|
| *anything_else*      | Der letzte Knoten des gesamten Dialogs. Dieser Knoten wird ausgeführt, wenn die Benutzereingabe keiner Absicht zugeordnet werden kann. |
| *context*            | Der Teil der verarbeiteten Dialognachricht mit dem JSON-Objekt. |
| *conversation_start* | Ein boolescher Wert, der in der ersten Runde des Dialogs mit 'true' ausgewertet wird (er kann in der Bedingung eines Dialogmodulknotens verwendet werden, um eine Willkommensnachricht für das Dialogmodul zu definieren). |
| *entities[ ]*        | Die Liste der Entitäten, die den Standardzugriff auf das erste Element unterstützen. |
| *input*              | Der Teil der verarbeiteten Dialognachricht mit dem JSON-Objekt. |
| *intents[ ]*         | Die Liste der Absichten, die den Standardzugriff auf das erste Element unterstützen. |
| *output*             | Der Teil der verarbeiteten Dialognachricht mit dem JSON-Objekt. |

## Auf Entitäten zugreifen

Das Entitätsarray enthält eine oder mehrere Entitäten.

Während Sie Ihr Dialogmodul testen, können Sie Details der Entitäten anzeigen, die in der Benutzereingabe erkannt wurden, indem Sie den folgenden Ausdruck in der Antwort eines Dialogmodulknotens angeben:

```json
<? entities ?>
```
{: codeblock}

Für die Benutzereingabe *Hello now* erkennt der Service die Systementitäten '@sys-date' und '@sys-time', weshalb die Antwort die folgenden Entitätsobjekte enthält:

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

### Kurzformsyntax für Entitäten

In der folgenden Tabelle sind Beispiele für die Kurzformsyntax aufgeführt, die Sie zum Referenzieren von Entitäten verwenden können:

| Kurzformsyntax      | Vollständige Syntax in SpEL              |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

In SpEL verhindert das Fragezeichen `(?)` das Auslösen einer Nullzeigerausnahme, wenn ein Entitätsobjekt null ist.

Falls der Entitätswert, den Sie überprüfen wollen, ein Zeichen `)` enthält, können Sie nicht den Operator `:` für Vergleiche verwenden. Falls Sie beispielsweise prüfen wollen, ob die Entität 'city' den Wert `Dublin (Ohio)` besitzt, müssen Sie `@city == 'Dublin (Ohio)'` anstelle von `@city:(Dublin (Ohio))` verwenden.

### Relevanz der Position von Entitäten in der Eingabe

Verwenden Sie den vollständigen SpEL-Ausdruck, wenn die Position von Entitäten in der Eingabe von Bedeutung ist. Die Bedingung `entities['city']?.contains('Boston')` gibt 'true' zurück, wenn mindestens eine Entität 'city' mit dem Wert 'Boston' unter allen Entitäten '@city' gefunden wird, wobei die Platzierung irrelevant ist.

Beispiel: Ein Benutzer übergibt `"I want to go from Toronto to Boston."` Die Entitäten `@city:Toronto` und `@city:Boston` werden erkannt und in den folgenden Entitäten dargestellt:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

Die Bedingung `@city.contains('Boston')` in einem Dialogmodulknoten gibt 'true' zurück, obwohl 'Boston' die zweite erkannte Entität ist.

### Entitätseigenschaften

Jeder Entität sind eine Reihe von Eigenschaften zugeordnet. Über die Eigenschaften können Sie auf Informationen zu einer Entität zugreifen.

| Eigenschaft           | Definition | Tipps zur Verwendung |
|-----------------------|------------|----------------------|
| *confidence*          | Ein als Dezimalzahl ausgedrückter Prozentsatz, der die Konfidenz des Service in der erkannten Entität darstellt. Die Konfidenz einer Entität ist entweder 0 oder 1, es sei denn, die unscharfe Suche für Entitäten ist aktiviert. Falls die unscharfe Suche aktiviert ist, liegt der Standardschwellenwert für das Konfidenzniveau bei 0,3. Systementitäten besitzen ungeachtet der Tatsache, ob die unscharfe Suche aktiviert ist oder nicht, immer das Konfidenzniveau 1,0. | Sie können diese Eigenschaft in einer Bedingung verwendet, damit 'false' zurückgegeben wird, wenn das Konfidenzniveau nicht höher als ein von Ihnen angegebener Prozentsatz ist. |
| *location*            | Eine relative Zeichenposition mit der Basis null, die angibt, wo der erkannte Entitätswert im Eingabetext beginnt und endet. | Mit `.literal` können Sie den Bereich des Textes zwischen den Start- und Endindexwerten extrahieren, die in der Eigenschaft 'location' gespeichert sind. |
| *value*               | Der in der Eingabe ermittelte Entitätswert. | Diese Eigenschaft gibt den Entitätswert so zurück, wie er in den Trainingsdaten definiert ist, und zwar auch dann, wenn die Übereinstimmung mit einem der zugehörigen Synonyme vorlag. Mit `.values` können Sie mehrere Vorkommen einer Entität erfassen, die möglicherweise in der Benutzereingabe enthalten sind. |

### Verwendungsbeispiele für Entitätseigenschaften
In den folgenden Beispielen enthält der Arbeitsbereich eine Entität 'airport', die den Wert 'JFK' und das Synonym 'Kennedy Airport' umfasst. Die Benutzereingabe lautet *I want to go to Kennedy Aiport*.

- Um eine bestimmte Antwort zurückzugeben, falls die Entität 'JFK' in der Benutzereingabe erkannt wird, könnten Sie den folgenden Ausdruck zur Antwortbedingung hinzufügen:
  `entities.airport[0].value == 'JFK'`
  oder
  `@airport = "JFK"`
- Um den Entitätsnamen so zurückzugeben, wie er vom Benutzer in der Dialogmodulantwort angegeben wurde, verwenden Sie die Eigenschaft '.literal':
  `So you want to go to <?entities.airport[0].literal?>...`
  oder
  `So you want to go to @airport.literal ...`

  Beide Formate werden in der Antwort mit 'So you want to go to Kennedy Airport...' ausgewertet.

- Ausdrücke wie `@airport:(JFK)` oder `@airport.contains('JFK')` referenzieren immer den **Wert** der Entität (im Beispiel `JFK`).
- Um die Erkennung von Begriffen als Flughäfen in der Eingabe strenger einzugrenzen, wenn die unscharfe Suche aktiviert ist, können Sie diesen Ausdruck in einer Knotenbedingung wie beispielsweise `@airport && @airport.confidence > 0.7` angeben. Der Knoten wird hier nur dann ausgeführt, wenn der Service mit einer Konfidenz von 70% ermittelt, dass die Eingabe einen Verweis auf einen Flughafen enthält.

Im Beispiel lautet die Benutzereingabe *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- Um mehrere Vorkommen eines Entitätstyps in einer Benutzereingabe zu erfassen, verwenden Sie eine Syntax wie die Folgende:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Um die erfasste Liste später in einer Dialogmodulantwort zu referenzieren, verwenden Sie die folgende Syntax:
  `You asked about these airports: <? $airports.join(', ') ?>.`
  Sie wird wie folgt angezeigt.
  `You asked about these airports: JFK, Logan, O'Hare.`

## Auf Absichten zugreifen

Das Absichtenarray enthält eine oder mehrere Absichten, die in der Benutzereingabe erkannt wurden und die in absteigender Reihenfolge gemäß ihrer Konfidenz sortiert sind. Jede Absicht besitzt nur eine einzige Eigenschaft, nämlich die Eigenschaft 'confidence'. Die Eigenschaft 'confidence' gibt einen als Dezimalzahl ausgedrückten Prozentsatz an, der die Konfidenz des Service bezüglich der erkannten Absicht darstellt.

Während Sie Ihr Dialogmodul testen, können Sie Details der Absichten anzeigen, die in der Benutzereingabe erkannt wurden, indem Sie den folgenden Ausdruck in der Antwort eines Dialogmodulknotens angeben:

```json
<? intents ?>
```
{: codeblock}

Für die Benutzereingabe *Hello now* hat der Service die Konfidenz ermittelt, dass die Absicht '#greeting' die beste Übereinstellung für die Benutzereingabe darstellt. Daher wird das Absichtsobjekt '#greeting' zuerst aufgelistet. Die Antwort enthält außerdem alle anderen Absichten, die im Arbeitsbereich definiert sind, auch wenn die Konfidenz bezüglich dieser Absichten so gering ist, dass sie auf 0 gerundet wird.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

Die folgenden Beispiele zeigen, wie Sie die Eingabe auf einen Absichtswert überprüfen können.

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` unterscheidet sich von `intents[0] == 'help'`, weil `intent == 'help'` keine Ausnahmebedingung auslöst, wenn keine Absicht erkannt wird. Eine Auswertung mit 'true' findet nur dann statt, wenn die Konfidenz der Absicht einen Schwellenwert überschreitet. Wenn Sie wollen, können Sie ein angepasstes Konfidenzniveau für eine Bedingung angeben, z. B. `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`.

### Kurzformsyntax für Absichten

In der folgenden Tabelle sind Beispiele für die Kurzformsyntax aufgeführt, die Sie zum Referenzieren von Absichten verwenden können:

| Kurzformsyntax          | Vollständige Syntax in SpEL          |
|-------------------------|--------------------------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` oder `#i_am_lost` | <code>(intent == 'help' &#124;&#124; intent == 'I_am_lost')</code> |

## Auf Eingabe zugreifen

Das JSON-Eingabeobjekt enthält eine einzige Eigenschaft, nämlich die Eigenschaft 'text'. Die Eigenschaft 'text' stellt den Text der Benutzereingabe dar.

### Verwendungsbeispiele für Eingabeeigenschaften

Das folgende Beispiel zeigt, wie auf die Eingabe zugegriffen wird:

- Um einen Knoten auszuführen, wenn die Benutzereingabe 'Yes' lautet, wird der folgende Ausdruck zur Knotenbedingung hinzugefügt:
  `input.text == 'Yes'`

Zur Auswertung oder Bearbeitung von Text aus der Benutzereingabe können Sie jede der [Methoden für Zeichenfolgen](/docs/services/conversation/dialog-methods.html#strings) verwenden. Beispiel:

- Um zu überprüfen, ob die Benutzereingabe 'Yes' enthält, verwenden Sie `input.text.contains( 'Yes' )`.
- Um 'true' zurückzugeben, wenn die Benutzereingabe eine Zahl ist, verwenden Sie `input.text.matches( '[0-9]+' )`.
- Um zu überprüfen, ob die Eingabezeichenfolge zehn Zeichen enthält, verwenden Sie `input.text.length() == 10`.

## Kurzformsyntax für Kontextvariablen

In der folgenden Tabelle sind Beispiele für die Kurzformsyntax aufgeführt, die Sie zum Schreiben von Kontextvariablen in Bedingungsausdrücken verwenden können.

| Kurzformsyntax             | Vollständige Syntax in SpEL             |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

In den Namen von Kontextvariablen können Sie Sonderzeichen wie Bindestriche oder Punkte verwenden. Dies kann allerdings zu Problemen bei der Auswertung des SpEL-Ausdrucks führen. Beispielsweise könnte ein Bindestrich als Minuszeichen interpretiert werden. Referenzieren Sie die Variable zur Vermeidung solcher Probleme entweder mit der vollständigen Ausdruckssyntax oder der Kurzformsyntax `$(variablenname)` und verwenden Sie im Namen keines der folgenden Sonderzeichen:

- Runde Klammern ()
- Mehr als ein Hochkomma ''
- Anführungszeichen "

## Auswertung

Um Variablenwerte innerhalb anderer Variablen zu erweitern oder um Methoden auf Variablen anzuwenden, verwenden Sie die Ausdruckssyntax `<? expression ?>`. Beispiel:

- **Eigenschaft erweitern**
    - `"output":{"text":"Your name is <? context.userName ?>"}`
    oder
    - `"output":{"text":"Your name is $userName"}` in Kurzformsyntax
- **Numerische Eigenschaft erhöhen**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Methoden (z. B. 'append') für Eigenschaften und globale Objekte aufrufen**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

### JSON-Objekt oder Zeichenfolgeformat

Wenn Sie die vollständige SpEL-Syntax in der Dialogmodulantwort verwenden, schließen Sie den Ausdruck in `<?` und `?>` ein, damit er im Zeichenfolgeformat wiedergegeben wird. Wenn Sie die vollständige SpEL-Syntax in einer Bedingung verwenden, nehmen Sie die einschließende Syntax `<? ?>`  nicht auf.

Falls Sie einen einzelnen SpEL-Ausdruck in einer Bedingung angeben, werden die Informationen im Objektformat zurückgegeben, damit der Ergebniswert seinen Datentyp beibehalten und in Gleichungen oder anderen Ausdrücken verwendet werden kann. Falls Sie mehrere SpEL-Ausdrücke in einer Bedingung angeben oder den Ausdruck als Teil einer Zeichenfolge angeben, werden die Informationen hingegen im Zeichenfolgeformat zurückgegeben. 

Beispielsweise können Sie den folgenden Ausdruck zu einem Dialogmodulknoten hinzufügen, damit die in der Benutzereingabe erkannten Entitäten zurückgegeben werden:

```json
The entities are <? entities ?>.
```
{: codeblock}

Falls der Benutzer *Hello now* als Eingabe angibt, werden die Entitätsinformationen im Zeichenfolgeformat bereitgestellt.

```json
The entities are 2017-08-07, 15:09:49.
```
{: codeblock}

Sie können den folgenden Ausdruck zur Antwort eines Dialogmodulknotens hinzufügen, damit die in der Benutzereingabe erkannten Absichten zurückgegeben werden:

```json
The intents are <? intents ?>.
```
{: codeblock}

Falls der Benutzer *Hello now* als Eingabe angibt, werden die Absichtsinformationen im Zeichenfolgeformat bereitgestellt.

```json
The intents are [
{"intent":"greeting","confidence":0.9331061244010925},
{"intent":"yes","confidence":0.06050306558609009},
{"intent":"pizza-order","confidence":0.052069634199142456},
...
]
```
{: codeblock}
