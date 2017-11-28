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

# Dialogmodul mithilfe der API ändern

Die REST-API von {{site.data.keyword.conversationshort}} unterstützt die programmgesteuerte Änderung von Dialogmodulen ohne Verwendung des {{site.data.keyword.conversationshort}}-Tools. Mit der API '/dialog_nodes' können Sie Dialogmodulknoten erstellen, löschen oder ändern.

Denken Sie daran, dass es sich bei einem Dialogmodul um eine Baumstruktur von vernetzten Knoten handelt, die bestimmte Regeln einhalten muss, damit sie gültig ist. Dies bedeutet, dass Änderungen, die Sie an einem Dialogmodulknoten vornehmen, Auswirkungen auf andere Knoten oder auf die Struktur Ihres Dialogmoduls nach sich ziehen können. Beschäftigen Sie sich daher unbedingt damit, wie Ihre Änderungen den Rest des Dialogmoduls beeinflussen, bevor Sie Ihr Dialogmodul mithilfe der API '/dialog_nodes' ändern.

Ein gültiges Dialogmodul erfüllt immer die folgenden Kriterien:

- Jeder Dialogmodulknoten besitzt eine eindeutige ID (Eigenschaft `dialog_node`).
- Jeder untergeordnete Knoten kennt seinen übergeordneten Knoten (Eigenschaft `parent`). Ein übergeordneter Knoten kennt jedoch nicht seine untergeordneten Knoten.
- Ein Knoten kennt, sofern vorhanden, seinen unmittelbar vorhergehenden gleichgeordneten Knoten (Eigenschaft `previous_sibling`). Dies bedeutet, dass alle gleichgeordneten Knoten eines bestimmten übergeordneten Knotens eine verkettete Liste bilden, in der jeder Knoten auf den vorherigen Knoten verweist.
- Nur ein einziger untergeordneter Knoten eines bestimmten übergeordneten Knotens kann der erste gleichgeordnete Knoten sein (hat also die Eigenschaft `previous_sibling` mit dem Wert 'null').
- Ein Knoten kann nicht auf einen vorherigen gleichgeordneten Knoten verweisen, der ein untergeordneter Knoten eines anderen übergeordneten Knotens ist.
- Zwei Knoten können nicht auf denselben vorherigen gleichgeordneten Knoten verweisen.
- Ein Knoten kann einen anderen Knoten angeben, der als Nächstes auszuführen ist (Eigenschaft `next_step`).
- Ein Knoten kann nicht sein eigener übergeordneter oder gleichgeordneter Knoten sein.
- Ein Knoten des Typs `response_condition` kann keine untergeordneten Knoten besitzen.

Die folgenden Beispiele zeigen, wie verschiedene Änderungen in der Folge zu nachgelagerten Änderungen führen können.

## Knoten erstellen
{: #create-node}

Als Beispiel wird die folgende Baumstruktur eines einfachen Dialogmoduls zugrunde gelegt:

![Beispiel für Dialogmodul](images/dialog_api_1.png)

Ein neuer Knoten kann durch Absetzen einer Anforderung POST für '/dialog_nodes' mit dem folgenden Hauptteil erstellt werden:

```json
{
  "dialog_node": "node_8"
}
```

Das Dialogmodul sieht danach wie folgt aus:

![Beispiel 2 für Dialogmodul](images/dialog_api_2.png)

Da der Knoten **node_8** ohne Angabe eines Wertes für `parent` oder `previous_sibling` erstellt wurde, ist er jetzt der erste Knoten im Dialogmodul. Der Service hat aber nicht nur den Knoten **node_8** erstellt, sondern auch den Knoten **node_1** so geändert, dass seine Eigenschaft `previous_sibling` auf den neuen Knoten verweist.

Durch die Angabe des übergeordneten und des vorherigen gleichgeordneten Knotens können Sie einen Knoten an einer beliebigen anderen Stelle im Dialogmodul erstellen:

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

Die Werte, die Sie für `parent` und `previous_node` angeben, müssen gültig sein:

- Beide Werte müssen vorhandene Knoten referenzieren.
- Der angegebene übergeordnete Knoten muss derselbe Knoten wie der übergeordnete Knoten des vorherigen gleichgeordneten Knoten sein (bzw. `null`, falls der vorherige gleichgeordnete Knoten keinen übergeordneten Knoten besitzt).
- Der übergeordnete Knoten kann nicht den Knotentyp `response_condition` besitzen.

Anschließend sieht das Dialogmodul so aus:

![Beispiel 3 für Dialogmodul](images/dialog_api_3.png)

Der Service hat nicht nur den Knoten **node_9**  erstellt, sondern auch die Eigenschaft `previous_sibling` des Knotens *node_6* so aktualisiert, dass sie auf den neuen Knoten verweist.

## Knoten zu einem anderen übergeordneten Knoten verschieben
{: #change-parent}

Der Knoten **node_5** soll jetzt zu einem anderen übergeordneten Knoten verschoben werden. Hierzu wird die Methode POST für '/dialog_nodes/node_5' mit dem folgenden Hauptteil übergeben:

```json
{
  "parent": "node_1"
}
```

Der angegebene Wert für `parent` muss gültig sein:
- Er muss einen vorhandenen Knoten referenzieren.
- Er darf nicht den Knoten referenzieren, der geändert wird (weil ein Knoten nicht sein eigenes übergeordnetes Element sein kann).
- Er darf nicht ein untergeordnetes Element des Knotens referenzieren, der geändert wird.
- Er darf keinen Knoten des Typs `response_condition` referenzieren.

Dies hat die folgende geänderte Struktur zum Ergebnis:

![Beispiel 4 für Dialogmodul](images/dialog_api_4.png)

Diese Änderung hatte mehrere Dinge zur Folge:
- Als der Knoten **node_5** zu seinem neuen übergeordneten Knoten verschoben wurde, wurde gleichzeitig auch der Knoten **node_7** verschoben (weil sich der Wert der Eigenschaft `parent` des Knotens **node_7** nicht geändert hat). Wenn Sie einen Knoten verschieben, bleiben alle untergeordneten Elemente dieses Knotens ihm weiterhin zugeordnet.
- Da für die Eigenschaft `previous_sibling` des Knotens **node_5** kein Wert angegeben wurde, ist dieser Knoten jetzt der erste gleichgeordnete Knoten unter dem Knoten **node_1**.
- Die Eigenschaft `previous_sibling` des Knotens **node_4** wurde mit dem Wert `node_5` aktualisiert.
- Die Eigenschaft `previous_sibling` des Knotens **node_9** wurde mit dem Wert `null` aktualisiert, weil es sich bei diesem Knoten jetzt um den ersten gleichgeordneten Knoten unter dem Knoten **node_2** handelt.

## Reihenfolge von gleichgeordneten Knoten ändern
{: #change-sibling}

Jetzt soll der Knoten **node_5** vom ersten zum zweiten gleichgeordneten Knoten gemacht werden. Hierzu wird die Methode POST für '/dialog_nodes/node_5' mit dem folgenden Hauptteil verwendet:

```json
{
  "previous_sibling": "node_4"
}
```

Wenn Sie die Eigenschaft `previous_sibling` ändern, muss der neue Wert gültig sein:
- Er muss einen vorhandenen Knoten referenzieren.
- Er darf nicht den Knoten referenzieren, der geändert wird (weil ein Knoten nicht sein eigenes gleichgeordnetes Element sein kann).
- Er muss ein übergeordnetes Element desselben übergeordneten Knotens referenzieren (alle gleichgeordneten Elemente müssen dasselbe übergeordnete Element besitzen).

Die Struktur ändert sich folgendermaßen:

![Beispiel 5 für Dialogmodul](images/dialog_api_5.png)

Auch hier bleib der Knoten **node_7** seinem übergeordneten Knoten zugeordnet. Außerdem wird der Knoten **node_4** so geändert, dass seine Eigenschaft `previous_sibling` den Wert `null` hat, weil er jetzt der erste gleichgeordnete Knoten ist.

## Knoten löschen
{: #delete-node}

Als Nächstes soll der Knoten **node_1** mit der Methode DELETE für '/dialog_nodes/node_1' gelöscht werden.

Hierdurch wird das folgende Ergebnis erzielt:

![Beispiel 6 für Dialogmodul](images/dialog_api_6.png)

Die Knoten **node_1**, **node_4**, **node_5** und **node_7** wurden sämtlich gelöscht. Wenn Sie einen Knoten löschen, werden auch alle ihm untergeordneten Knoten gelöscht. Falls Sie einen Basisknoten löschen, wird somit tatsächlich eine ganze Verzweigung in der Baumstruktur des Dialogmoduls gelöscht. Alle anderen Referenzen auf den gelöschten Knoten (z. B. Referenzen in `next_step`) werden in `null` geändert.

Darüber hinaus wurde durch diese Aktion der Knoten **node_2** aktualisiert und verweist nun auf den Knoten **node_8** als seinen neuen vorherigen gleichgeordneten Knoten.

## Knoten umbenennen
{: #rename-node}

Zum Schluss soll der Knoten **node_2** unter Verwendung der Methode POST für '/dialog_nodes/node_2' mit dem folgenden Hauptteil umbenannt werden:

```json
{
  "dialog_node": "node_X"
}
```

![Beispiel 7 für Dialogmodul](images/dialog_api_7.png)

Die Struktur des Dialogmoduls hat sich hierdurch zwar nicht geändert, aber auch in diesem Fall wurden mehrere Knoten geändert, um den neuen Namen widerzuspiegeln:

- Die Eigenschaften `parent` der Knoten **node_9** und **node_6** wurden geändert.
- Die Eigenschaft `previous_sibling` des Knotens **node_3** wurde geändert.

Alle anderen Referenzen auf den gelöschten Knoten (z. B. Referenzen in `next_step`) wurden ebenfalls geändert.
