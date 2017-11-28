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

# Absichten definieren

***Absichten*** sind Zwecke oder Ziele, die in der Eingabe eines Kunden ausgedrückt werden, beispielsweise die Beantwortung einer Frage oder die Verarbeitung einer Rechnungszahlung. Indem der Service '{{site.data.keyword.conversationshort}}' die in einer Kundeneingabe ausgedrückte Absicht erkennt, kann er den korrekten Dialogmodulablauf für ihre Beantwortung auswählen.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VG0YykNcfv8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Begrenzungen für Absichten
{: #intent-limits}

Die Anzahl der Absichten und Beispiele, die Sie erstellen können, richtet sich nach Ihrem {{site.data.keyword.conversationshort}}-Serviceplan:

| Serviceplan      | Absichten pro Arbeitsbereich | Beispiele pro Arbeitsbereich |
|------------------|-----------------------------:|-----------------------------:|
| Standard/Premium |                        2,000 |                       25,000 |
| Lite             |                           25 |                       25,000 |

## Absichten erstellen
{: #creating-intents}

Zum Erstellen von Absichten verwenden Sie das {{site.data.keyword.conversationshort}}-Tool. 

1.  Öffnen Sie Ihren Arbeitsbereich im {{site.data.keyword.conversationshort}}-Tool und wählen Sie dann die Registerkarte **Absichten** in der Navigationsleiste aus. Falls die Registerkarte **Absichten** nicht angezeigt wird, öffnen Sie die Seite über das Menü ![Menü](images/Menu_16.png).
1.  Wählen Sie **Neue erstellen** aus.
1.  Geben Sie im Feld **Name der Absicht** einen beschreibenden Namen für die Absicht ein.
    - Der Name der Absicht kann Buchstaben (in Unicode), Ziffern, Unterstreichungszeichen, Bindestriche und Punkte enthalten.
    - Der Name kann aus `..` oder einer beliebigen anderen Zeichenfolge ausschließlich mit Zeichen bestehen.
    - Namen von Absichten dürfen keine Leerzeichen enthalten und nicht länger als 128 Zeichen sein. Beispiele für Namen von Absichten:
        - `#wetterlage`
        - `#rechnung_zahlen`
        - `#eskalation_an_agenten`

    Das Tool bezieht automatisch das Zeichen `#` in den Namen einer Absicht ein; Sie müssen das Zeichen also nicht selbst hinzufügen.
    {: tip}

    Durch Auswahl von **Erstellen** können Sie den Namen der Absicht speichern, ohne Beispiele hinzuzufügen. Sie können aber auch das Feld 'Benutzerbeispiel' auswählen oder mit der Tabulatortaste weiterspringen und Beispiele hinzufügen.

1.  Geben Sie im Feld **Benutzerbeispiel** den Text eines Benutzerbeispiels für die Absicht ein. Ein Beispiel kann bis zu 1024 Zeichen lang sein. Mögliche Beispiele für die Absicht `#rechnung_zahlen`:
    - `Ich muss meine Rechnung zahlen.`
    - `Kontostand ausgleichen`
    - `Zahlung vornehmen`

    Wenn Sie bereits Entitäten definiert haben, die dieser Absicht entsprechen, bzw. solche Entitäten definieren wollen, verwenden Sie die Entitäten oder ihre zugeordneten Synonyme in einigen Beispielen. Dies fördert den Aufbau einer Beziehung zwischen der Absicht und den Entitäten.

    > **Wichtig:** Namen von Absichten und Beispieltext können in URLs zugänglich sein, wenn eine Anwendung mit dem Service interagiert. Verwenden Sie in diesen Artefakten keine vertraulichen oder persönlichen Daten.

    Drücken Sie die Eingabetaste oder wählen Sie das Symbol **+** aus, um das Beispiel zu speichern.
1.  Wiederholen Sie diesen Prozess, um weitere Beispiele hinzuzufügen. Mit der Tabulatortaste können Sie zwischen den einzelnen Beispielen wechseln. Geben Sie für jede Absicht mindestens fünf Beispiele an. Je mehr Beispiele Sie angeben, desto präziser kann die Anwendung arbeiten.

    ![Screenshot, der die Definition einer Absicht zeigt.](images/define_intent.png)
1.  Nachdem Sie die Beispiele hinzugefügt haben, wählen Sie **Fertig** aus, um die Erstellung der Absicht abzuschließen.

### Ergebnisse

Die von Ihnen erstellte Absicht wird zur Registerkarte 'Absichten' hinzugefügt. Das System beginnt nun damit, sich selbst mit den neuen Daten zu trainieren.

## Absichten bearbeiten

Sie können jede Absicht in der Liste öffnen und sie bearbeiten. Hierbei können Sie die folgenden Änderungen vornehmen:

- Sie können die Absicht umbenennen.
- Sie können die Absicht löschen.
- Sie können Beispiele hinzufügen, bearbeiten oder löschen.
- Sie können ein Beispiel in eine andere Absicht verschieben.

Mit der Tabulatortaste können Sie vom Namen der Absicht zu den einzelnen Beispielen springen und die Beispiele bei Bedarf bearbeiten.

Um ein Beispiel zu verschieben, wählen Sie es mithilfe des Kontrollkästchens aus und wählen Sie dann **Verschieben nach** aus.

  ![Screenshot, der das Verschieben eines Beispiels zeigt.](images/move_example.png)

## Absichten und Beispiele importieren

Bei einer großen Anzahl von Absichten und Beispielen kann es einfacher sein, diese aus einer CSV-Datei zu importieren, als sie einzeln im {{site.data.keyword.conversationshort}}-Tool zu definieren.

1.  Erfassen Sie die Absichten und Beispiele in einer CSV-Datei oder exportieren Sie sie aus einem Tabellenkalkulationsprogramm in eine CSV-Datei. Jede Zeile in der Datei muss das folgende erforderliche Format aufweisen:

    ```
    <beispiel>,<absicht>
    ```
    {: screen}

    Hierbei steht `<beispiel>` für den Text eines Benutzerbeispiels und `<absicht>` für den Namen der Absicht, zu der das Beispiel gehören soll. Beispiel:

    ```
    Ich möchte Informationen zur aktuellen Wetterlage.,wetterlage
    Regnet es?,wetterlage
    Wie hoch ist die Temperatur?,wetterlage
    Wo ist Ihre nächste Filiale?,filialsuche
    Haben Sie einen Laden in Berlin?,filialsuche
    ```
    {: screen}

    > **Wichtig:** Speichern Sie die CSV-Datei in UTF-8-Codierung und ohne Byteanordnungsmarkierung.

1.  Öffnen Sie Ihren Arbeitsbereich im {{site.data.keyword.conversationshort}}-Tool und wählen Sie dann die Registerkarte **Absichten** in der Navigationsleiste aus. Falls die Registerkarte **Absichten** nicht angezeigt wird, öffnen Sie die Seite über das Menü ![Menü](images/Menu_16.png).

1.  Wählen Sie das Symbol ![Importieren](images/importGA.png) aus und ziehen Sie dann eine Datei. Suchen Sie alternativ auf Ihrem Computer nach einer Datei und wählen Sie sie aus. Die Datei wird validiert und importiert. Anschließend beginnt das System damit, sich selbst mit den neuen Daten zu trainieren.

    > **Wichtig:** Die maximale Größe der CSV-Datei beträgt 10 MB. Wenn Ihre CSV-Datei größer ist, können Sie sie in mehrere Dateien aufteilen und diese dann einzeln importieren.

### Ergebnisse

Sie können die importierten Absichten und die entsprechenden Beispiele auf der Registerkarte 'Absichten' einsehen. Möglicherweise müssen Sie die Seite aktualisieren, damit die neuen Absichten und Beispiele angezeigt werden.

## Absichten exportieren
{: #export_intents}

Sie können Absichten in eine CSV-Datei exportieren, um sie anschließend zu importieren und für eine andere Conversation-Anwendung wiederzuverwenden.

1.  Wählen Sie auf der Registerkarte 'Absichten' das Symbol ![Exportieren](images/ExportIcon.png) aus.

    ![Optionen 'Exportieren' und 'Löschen'](images/ExportIntent1.png)

1.  Wählen Sie die gewünschten Absichten aus und klicken Sie auf die Schaltfläche **Exportieren**.
    ![Entitätsauswahl für Schaltflächen 'Exportieren' und 'Löschen'](images/ExportIntent2.png)

## Absichten löschen
{: #delete_intents}

Sie können eine gewünschte Anzahl von Absichten auswählen, um sie anschließend zu löschen.

**WICHTIG**: Beim Löschen von Absichten werden auch alle zugehörigen Beispiele gelöscht. Diese Einträge können später nicht mehr abgerufen werden. Alle Dialogmodulknoten, die diese Absichten referenzieren, müssen manuell aktualisiert werden, damit der gelöschte Inhalt nicht mehr referenziert wird.

1.  Wählen Sie auf der Registerkarte 'Absichten' das Symbol ![Löschen](images/DeleteIcon.png) aus.

    ![Optionen 'Exportieren' und 'Löschen'](images/ExportIntent3.png)

1.  Wählen Sie die Absichten aus, die Sie löschen wollen, und klicken Sie auf die Schaltfläche **Löschen**. **Hinweis**: Die Löschfunktion unterstützt die Massenlöschung von Absichten. 

## Absichten testen
{: #testing-your-intents}

Nachdem Sie die neuen Absichten erstellt haben, können Sie das System testen und feststellen, ob es Ihre Absichten wie erwartet erkennt.

1.  Wählen Sie im {{site.data.keyword.conversationshort}}-Tool das Symbol ![Watson fragen](images/ask_watson.png) aus.

1.  Geben Sie in der Anzeige 'Ausprobieren' eine Frage oder eine andere Textzeichenfolge ein und drücken Sie die Eingabetaste, um zu ermitteln, welche Absicht erkannt wird. Sollte die falsche Absicht erkannt werden, können Sie Ihr Modell verbessern, indem Sie den entsprechenden Text als Beispiel zur passenden Absicht hinzufügen.

    Falls Sie kürzlich Änderungen im Arbeitsbereich vorgenommen haben, wird möglicherweise in einer Nachricht darauf hingewiesen, dass das System noch mit dem erneuten Training beschäftigt ist. Wenn diese Nachricht ausgegeben wird, warten Sie mit dem Test, bis das Training abgeschlossen ist:
    {: tip}

    ![Screenshot mit Nachricht für erneutes Training](images/training.png)

    Die Antwort gibt Aufschluss darüber, welche Absicht aus Ihrer Eingabe erkannt wurde.

    ![Screenshot für den Test von Absichten](images/test_intents.png)

1.  Falls das System nicht die richtige Absicht erkannt hat, können Sie sie korrigieren. Zur Korrektur der erkannten Absicht wählen Sie die angezeigte Absicht und anschließend die richtige Absicht in der Liste aus. Nachdem Sie die Korrektur übergeben haben, trainiert sich das System automatisch selbst, um die neuen Daten zu integrieren. 

    ![Screenshot für Korrektur einer erkannten Absicht](images/correct_intent.png)

1.  Falls sich die Eingabe nicht auf Ihre Anwendung bezieht, können Sie dies angeben. Wählen Sie die angezeigte Absicht und dann die Option **Als irrelevant markieren** aus.

    ![Screenshot für Option 'Als irrelevant markieren'](images/irrelevant.png)

Wenn Ihre Absichten nicht korrekt erkannt werden, kann es sinnvoll sein, die folgenden Änderungen vorzunehmen:

- Fügen Sie den nicht erkannten Text als Beispiel zur entsprechenden Absicht hinzu.
- Verschieben Sie vorhandene Beispiele von einer Absicht zu einer anderen Absicht.
- Denken Sie darüber nach, ob Ihre Absichten zu ähnlich sind, und definieren Sie sie entsprechend neu.

## Absolute Bewertung und Option 'Als irrelevant markieren'

Seit Februar 2017 wird zur Bewertung der Absichtskonfidenz und zur Rückgabe von Absichten ein neuer Algorithmus eingesetzt. Außerdem können Sie Absichten als 'irrelevant' markieren. Diese Änderungen machen möglicherweise ein [Upgrade des Arbeitsbereichs ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](upgrading.html){: new_window} erforderlich.

### Absolute Bewertung

Die Konfidenz jeder Absicht wird jetzt separat und nicht in Relation zu anderen Absichten bewertet. Dies bietet die Flexibilität, dass mehrere Absichten zurückgegeben werden können. Außerdem bedeutet es, dass das System möglicherweise auch gar keine Absichten zurückgibt. Falls das System eine geringe Konfidenz (weniger als 0,2) dafür ermittelt, dass sich Absichten auf die Benutzereingabe beziehen, gibt das System keine Absicht zurück.

Wenn sich die Konfidenzbewertung von Absichten ändert, müssen Ihre Dialogmodule möglicherweise neu strukturiert werden. Beispiel: Sie haben für Ihr Dialogmodul eine Absicht als Bedingung festgelegt, für die eine geringe Konfidenz festgestellt wird. In diesem Fall ist die Antwort des Systems nicht mehr richtig.

### Als irrelevant markieren
{: #mark-irrelevant}

Ob dieses Feature für Sie verfügbar ist, können Sie im Abschnitt [Unterstützte Sprachen](lang-support.html) ermitteln.

Nach einem Upgrade Ihres Arbeitsbereichs können Sie in der Anzeige 'Ausprobieren' die [Eingabe testen](#testing-your-intents), um die Änderungen festzustellen. Mit der Option 'Als irrelevant markieren' können Sie angeben, dass die Eingabe für Ihre Anwendung nicht von Bedeutung ist.

Wenn Sie eine Absicht wie beispielsweise '#ohne_bedeutung' für Eingaben definiert haben, die den Rahmen sprengen oder unpassend sind, löschen Sie die Absicht und testen Sie Ihren Arbeitsbereich, indem Sie die Eingaben als irrelevant markieren.

> **Wichtig**: Als irrelevant markierte Eingaben werden im Arbeitsbereich gespeichert und in die Trainingsdaten einbezogen. Nehmen Sie diese Änderung nur dann vor, wenn Sie dies wirklich beabsichtigen.
> - Auf die Eingaben können Sie später im Tool nicht mehr zugreifen; auch eine Änderung ist dann nicht mehr möglich.
> - Die Kennzeichnung als irrelevant kann einzig dadurch entfernt werden, dass dieselbe Eingabe in der Anzeige 'Ausprobieren' verwendet und dann die Absicht geändert wird.
