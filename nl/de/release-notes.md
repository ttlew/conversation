---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-27"

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

# Releaseinformationen

## Versionierung der Service-API
{: shortdesc}

API-Anforderungen erfordern einen Parameter 'version', der das Datum im Format `version=JJJJ-MM-TT` annimmt. Bei jeder abwärtskompatiblen Änderung der API wird eine neue untergeordnete Version der API freigegeben.

Senden Sie den Parameter 'version' in jeder API-Anforderung. Der Service verwendet die API-Version des von Ihnen angegebenen Datums oder die letzte Version vor diesem Datum. Verwenden Sie nicht standardmäßig das aktuelle Datum. Geben Sie stattdessen ein Datum an, das mit einer Version übereinstimmt, die mit Ihrer App kompatibel ist, und ändern Sie es erst dann, wenn Ihre App für eine neuere Version bereit ist.

Die aktuelle Version ist `2017-05-26`.

## Features als Betaversion

IBM gibt Services, Features und Sprachunterstützung frei, die als 'Betaversion' (zuvor _experimentell_) klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach einer kurzen Benachrichtigung eingestellt werden. Unterstützung für Features als Betaversion ist nur über das {{site.data.keyword.Bluemix_notm}}-Forum verfügbar.

## Aktualisierte Modelle
{: #updated-models}

Die {{site.data.keyword.conversationshort}}-Algorithmen können aufgrund von Rückmeldungen, wissenschaftlichen Verbesserungen und weiteren Faktoren regelmäßig optimiert und aktualisiert werden, um die Leistung kontinuierlich zu verbessern. Aktualisierungen des Modells werden in den vorliegenden Releaseinformationen mitgeteilt.

Vorhandene Modelle, die Sie trainiert haben, sind nicht unmittelbar betroffen. Abgelaufene Modelle werden jedoch 60 Tage nach Beginn der Verfügbarkeit eines neuen Modells auf das aktuelle Modell aktualisiert, wenn Sie dies bis dahin nicht ausgeführt haben.

> **Hinweis:** Diese Aktualisierungsaussage gilt nur für Sprachen und Features mit dem Status 'GA' (= allgemein verfügbar).

## Änderungen
{: #change-log}

Die folgenden neuen Features und Änderungen am Service sind verfügbar.

### 26. Oktober 2017
{: #26October2017}

- **Aktualisierungen für vereinfachtes Chinesisch:** Die Sprachunterstützung für vereinfachtes Chinesisch wurde verbessert. Dies beinhaltet Verbesserungen für die Absichtsklassifizierung mithilfe von Worteinbettungen auf Zeichenebene und die Verfügbarkeit von Systementitäten. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).
- **Aktualisierungen für Spanisch:** Die Absichtsklassifizierung für Spanisch wurde für sehr große Datensätze verbessert.

### 11. Oktober 2017
{: #11October2017}

- **Aktualisierungen für Koreanisch:** Die Sprachunterstützung für Koreanisch wurde verbessert. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 3. Oktober 2017
{: #3October2017}

- **Musterdefinierte Entitäten (Betaversion):** Sie können nun unter Verwendung von regulären Ausdrücken bestimmte Muster für eine Entität definieren. Dies kann für die Ermittlung von Entitäten von Nutzen sein, die ein definiertes Muster befolgen, z. B. Artikel- bzw. Teilenummern, Telefonnummern oder E-Mail-Adressen. Weitere Details enthält der Abschnitt [Musterdefinierte Entitäten](entities.html#pattern-entities).
    - Sie können entweder Synonyme oder Muster für einen einzelnen Entitätswert hinzufügen, jedoch nicht beides.
    - Für jeden Entitätswert kann es höchstens 5 Muster geben.
    - Jedes Muster (also jeder reguläre Ausdruck) ist auf 128 Zeichen begrenzt.
    - Beim Importieren oder Exportieren über eine CSV-Datei werden Muster gegenwärtig nicht unterstützt.
    - Die REST-API unterstützt keinen direkten Zugriff auf Muster. Sie können Muster jedoch mit dem Endpunkt `/values` abrufen oder ändern.

- **Nach Wörterverzeichnis gefilterte unscharfe Suche (nur bei Englisch):** Für Englisch gibt es jetzt eine verbesserte Version der unscharfen Suche für Entitäten. Diese Verbesserung verhindert die Erfassung einiger gängiger und gültiger englischer Wörter als grobe Übereinstimmungen für eine bestimmte Entität. Bei der unscharfen Suche wird beispielsweise der Entitätswert `like` nicht als Übereinstimmung für die gültigen englischen Wörter `hike` oder `bike` gewertet, jedoch weiterhin beispielsweise für `lkie` oder `oike`.

### 27. September 2017
{: #27September2017}

- **Aktualisierungen beim Bedingungsbuilder:** Das Steuerelement, das angezeigt wird, um Sie beim Definieren einer Bedingung in einem Dialogmodulknoten zu unterstützen, wurde aktualisiert. Zu den Erweiterungen zählen die Unterstützung der Auflistung von verfügbaren Kontextvariablennamen, nachdem Sie das Zeichen $ eingegeben haben, um eine Kontextvariable hinzuzufügen.

### 31. August 2017
{: #31August2017}

- **Verbesserung des Abschnittsrollbacks:** Die Metrik für die gemittelte Dialogzeit und entsprechende Filter wurden vorübergehend von der Seite 'Übersicht' des Abschnitts 'Verbessern' entfernt. Dies verhindert, dass die Berechnung bestimmter Metriken die Metrik für die gemittelte Dialogzeit verursacht und im Diagramm für Dialoge über einen bestimmten Zeitraum hinweg ungenaue Informationen angezeigt werden. IBM bedauert das Entfernen von Funktionalität aus dem Tool, setzt hierdurch jedoch seine Zusage um, die Bereitstellung von präzisen Informationen für die Benutzer sicherzustellen.
- **Namen von Dialogmodulknoten:** Einem Dialogmodulknoten kann nun ein beliebiger Name zugeordnet werden und der Name muss nicht eindeutig sein. Außerdem kann der Knotenname nachfolgend geändert werden, ohne dass hierdurch die interne Referenzierung des Knotens beeinflusst wird. Der Name, den Sie angeben, wird als Attribut 'title' des Knotens in der JSON-Datei für den Arbeitsbereich gespeichert; das System referenziert den Knoten mithilfe einer eindeutigen ID, die im Attribut 'name' gespeichert wird.

### 23. August 2017
{: #23August2017}

- **Aktualisierungen für Koreanisch, Japanisch und Italienisch:** Die Sprachunterstützung für Koreanisch, Japanisch und Italienisch wurde verbessert. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 10. August 2017
{: #10August2017}

- **Akzentnormalisierung:** In einer dialogorientierten Einstellung können Benutzer bei der Interaktion mit dem Service 'Conversation' Akzente verwenden oder nicht. Insofern wurde eine Aktualisierung des Algorithmus vorgenommen, damit Fassungen von Wörtern mit Akzent und ohne Akzent bei der Absichts- und Entitätserkennung identisch behandelt werden.

  Bei einigen Sprachen (z. B. Spanisch) können manche Akzente jedoch die Bedeutung der Entität ändern. Obwohl die Originalentität implizit einen Akzent besitzt, kann der Service daher bei der Entitätserkennung auch eine Übereinstimmung mit der Version derselben Entität ohne Akzent erzielen, allerdings mit einer etwas geringeren Konfidenzbewertung.

  Beispiel: Für das Wort 'barrió' mit Akzent, das der Vergangenheitsform des Verbs 'barrer' (fegen) entspricht, kann der Service auch eine Übereinstimmung mit dem Wort 'barrio' (Nachbarschaft) ermitteln, jedoch mit einer etwas geringeren Konfidenzbewertung.

  Das System stellt die höchsten Konfidenzbewertungen in Entitäten mit exakten Übereinstimmungen bereit. Beispielsweise wird `barrio` nicht erkannt, wenn `barrió` im Trainingsset angegeben ist, und `barrió` wird nicht erkannt, wenn `barrio` im Trainingsset enthalten ist.

  Sie müssen dafür sorgen, dass das System mit den richtigen Zeichen und Akzenten trainiert wird. Falls Sie beispielsweise `barrió` als Antwort erwarten, müssen Sie `barrió` in das Trainingsset aufnehmen.

  Obwohl es sich streng genommen bei der Tilde nicht um ein Akzentzeichen handelt, gilt dasselbe für Wörter, die z. B. den spanischen Buchstaben `ñ` im Gegensatz zum Buchstaben `n` verwenden (z. B. 'uña' und 'una'). In diesem Fall ist der Buchstabe `ñ` nicht einfach ein `n` mit einem Akzent, sondern ein eindeutiger besonderer Buchstabe des Spanischen.

  Sie können die unscharfe Suche aktivieren, wenn Sie vermuten, dass Ihre Kunden möglicherweise nicht die richtigen Akzente verwenden oder Wörter falsch schreiben (beispielsweise `n` statt `ñ` verwenden). Sie können die Varianten aber auch explizit in die Trainingsbeispiele aufnehmen.

  **Hinweis:** Die Akzentnormalisierung ist für Portugiesisch, Spanisch, Französisch und Tschechisch aktiviert.

- **Flag zum Ablehnen von Arbeitsbereichen:** Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt ein Flag für die Ablehnung von Arbeitsbereichen. Dieses Flag gibt an, dass Trainingsdaten für den Arbeitsbereich wie Absichten und Entitäten nicht für allgemeine Serviceverbesserungen durch IBM verwendet werden sollen. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}.

### 7. August 2017
{: #7August2017}

- **Interpretation für Datumsangaben mit `nächste/r/s` und `letzte/r/s`:** Der Service 'Conversation' behandelt Datumsangaben mit 'letzte/r/s' und 'nächste/r/s' so, als ob sich diese auf den unmittelbar letzten oder nächsten Tag beziehen, der in derselben, aber auch in einer vorherigen Woche liegen kann. Weitere Informationen enthält der Abschnitt [Systementitäten](system-entities.html#sys-datetime).

### 3. August 2017
{: #3August2017}

- **Unscharfe Suche für zusätzliche Sprachen (Betaversion):** Die unscharfe Suche für Entitäten ist jetzt für weitere Sprachen verfügbar (siehe [Unterstützte Sprachen](lang-support.html)).
- **Suche mit teilweiser Übereinstimmung (Betaversion, nur Englisch):** Bei der unscharfen Suche werden jetzt automatisch auf Teilzeichenfolgen basierende Synonyme vorgeschlagen, die in benutzerdefinierten Entitäten vorhanden sind, und im Vergleich zur exakten Übereinstimmung für die Entität wird eine geringere Konfidenzbewertung zugeordnet. Details finden Sie unter [Unscharfe Suche](entities.html#fuzzy-matching).

### 28. Juli 2017
{: #28July2017}

- Wenn Sie für das Tool Vorgaben für bidirektionale Sprachen festlegen, können Sie nun die Richtung der grafischen Benutzerschnittstelle angeben.
- Das Toolfarbschema wurde aktualisiert und ist nun mit anderen Watson-Services und -Produkten konsistent.

### 19. Juli 2017
{: #19July2017}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt den Zugriff auf Dialogmodulknoten. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14. Juli 2017
{: #14July2017}

- Die Slotfunktionalität von Dialogmodulen wurde erweitert. Beispielsweise wurde eine Eigenschaft *slot_in_focus* hinzugefügt, damit Sie eine Bedingung definieren können, die nur auf einen einzigen Slot angewendet wird. Details können Sie dem Abschnitt [Informationen mit Slots erfassen](/docs/services/conversation/dialog-build.html#slots) entnehmen.

### 12. Juli 2017
{: #12July2017}

- **Unterstützung für Tschechisch:** Die Unterstützung der tschechischen Sprache wurde eingeführt. Weitere Details enthält der Abschnitt [Unterstützte Sprachen](lang-support.html).

### 11. Juli 2017
{: #11July2017}

- **In Slack testen:** Mit dem neuen Tool **In Slack testen** können Sie Ihren Arbeitsbereich jetzt ohne großen Aufwand zu Testzwecken als Slack-Bot-Benutzer bereitstellen. Dieses Tool ist nur für die {{site.data.keyword.Bluemix_notm}}-Region 'US South' verfügbar. Weitere Informationen finden Sie unter [In Slack testen](test-deploy.html).
- **Aktualisierungen für Arabisch:** Die Unterstützung der arabischen Sprache wurde erweitert und beinhaltet jetzt die absolute Bewertung pro Absicht sowie die Möglichkeit, Absichten als irrelevant zu markieren; zusätzliche Angaben enthält der Abschnitt [Unterstützte Sprachen](lang-support.html). Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 23. Juni 2017
{: #23June2017}

- **Aktualisierungen für Koreanisch:** Die Unterstützung der koreanischen Sprache wurde erweitert; zusätzliche Angaben enthält der Abschnitt [Unterstützte Sprachen](lang-support.html). Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 22. Juni 2017
{: #22June2017}

- **Einführung von Slots:** Die Erfassung mehrerer Einzelinformation von einem Benutzer in einem einzigen Knoten wird jetzt durch das Hinzufügen von Slots vereinfacht. Zuvor mussten mehrere Dialogmodulknoten erstellt werden, um alle möglichen Kombinationen der Möglichkeiten zu berücksichtigen, die Benutzer bei der Bereitstellung von Informationen haben. Mithilfe von Slots können Sie einen einzigen Knoten konfigurieren, der alle vom Benutzer bereitgestellten Informationen speichert und alle benötigten Details anfordert, die vom Benutzer nicht angegeben wurden. Weitere Details enthält der Abschnitt [Informationen mit Slots erfassen](dialog-build.html#slots).
- **Vereinfachte Baumstruktur von Dialogmodulen:** Die Baumstruktur von Dialogmodulen wurde überarbeitet, um den Bedienungskomfort zu verbessern. Die Baumstrukturansicht ist jetzt kompakter und Sie können einfacher erkennen, an welcher Stelle Sie sich in der Baumstruktur befinden. Außerdem werden die Links zwischen Knoten jetzt so dargestellt, dass Sie die Beziehungen zwischen den Knoten leichter verstehen können.

### 21. Juni 2017
{: #21June2017}

- **Unterstützung für Arabisch:** Die Unterstützung für Arabisch ist jetzt allgemein verfügbar. Details enthält der Abschnitt [Bidirektionale Sprachen konfigurieren](lang-support.html#configuring-bi-directional).
- **Aktualisierungen für Sprachen:** Der Algorithmus des Service '{{site.data.keyword.conversationshort}}' wurde aktualisiert, um die gesamte Sprachunterstützung zu verbessern. Einzelheiten enthält der Abschnitt [Unterstützte Sprachen](lang-support.html).

### 16. Juni 2017
{: #16June2017}

- **Empfehlungen (Betaversion, nur für Premium-Benutzer):** Die Anzeige 'Verbessern' enthält jetzt auch eine Seite **Empfehlungen**, auf der Verfahren vorgeschlagen werden, mit denen Sie Ihr System verbessern können. Hierzu werden die Dialoge von Benutzern mit Ihrem Chat-Bot analysiert und die aktuellen Trainingsdaten sowie die gegenwärtige Antwortsicherheit Ihres Systems berücksichtigt.

### 14. Juni 2017
{: #14June2017}

- **Unscharfe Suche für zusätzliche Sprachen (Betaversion):** Die unscharfe Suche für Entitäten ist jetzt für weitere Sprachen verfügbar (siehe [Unterstützte Sprachen](lang-support.html)). Durch eine Aktivierung der unscharfen Suche für eine jeweilige Entität können Sie die Fähigkeit des Service verbessern, Benutzereingabeterms mit einer Syntax zu erkennen, die Ähnlichkeit mit der Entität hat, ohne dass eine exakte Übereinstimmung erforderlich ist. Das Feature kann die Benutzereingabe der entsprechenden Entität trotz einer falschen Schreibweise oder leichten syntaktischen Abweichungen zuordnen. Wenn Sie beispielsweise 'Giraffe' als Synonym einer Entität für ein Tier definieren und die Benutzereingabe den Begriff 'Giraffen' oder Girafe' enthält, kann die unscharfe Suche den Begriff korrekt zur Entität für das Tier zuordnen. Details finden Sie unter [Unscharfe Suche](entities.html#fuzzy-matching).

### 13. Juni 2017
{: #13June2017}

- **Benutzerdialoge:** Die Anzeige 'Verbessern' enthält jetzt eine Seite **Benutzerdialoge** mit einer Liste der Benutzerinteraktionen mit Ihrem Chat-Bot, die nach Schlüsselwort, Absicht, Entität oder Anzahl von Tagen gefiltert werden kann. Sie können einzelne Dialoge öffnen, um Absichten zu korrigieren oder um Entitätswerte bzw. Synonyme hinzuzufügen.
- **Änderung für reguläre Ausdrücke:** Die regulären Ausdrücke, die von SpEL-Funktionen unterstützt werden (z.  B. find, matches, extract, replaceFirst, replaceAll und split) wurden geändert. Gruppen von Konstrukten mit regulären Ausdrücken sind nicht mehr zulässig, hierzu zählen Konstrukte mit 'look-ahead', 'look-behind', possessiver Wiederholung und Rückverweisen. Diese Änderung war erforderlich, um ein Sicherheitsrisiko in der ursprünglichen Bibliothek für reguläre Ausdrücke zu verhindern.

### 12. Juni 2017
{: #12June2017}

- Die maximale Anzahl von Arbeitsbereichen, die Sie mit dem Plantyp **Lite** (zuvor 'Free' genannt) erstellen können, wurde von 3 auf 5 erhöht.
- Sie können künftig einem Dialogmodulknoten einen beliebigen Namen zuordnen, der nicht eindeutig sein muss. Außerdem kann der Knotenname nachfolgend geändert werden, ohne dass hierdurch die interne Referenzierung des Knotens beeinflusst wird. Der von Ihnen angegebene Name wird als Aliasname behandelt; das System verwendet eine eigene interne ID, um den Knoten zu referenzieren.
- Die Sprache eines Arbeitsbereichs kann künftig nicht mehr nach seiner Erstellung durch eine Bearbeitung der Arbeitsbereichsdetails geändert werden. Falls Sie die Sprache ändern müssen, können Sie den Arbeitsbereich als JSON-Datei exportieren, die Eigenschaft 'language' aktualisieren und dann die JSON-Datei als neuen Arbeitsbereich importieren.

### 6. Juni 2017
{: #6June2017}

- **Weitere Informationen:** Es gibt eine neue Seite mit *Informationen zu {{site.data.keyword.watson}} {{site.data.keyword.conversationshort}}*, die einführende Informationen und Links zur Servicedokumentation sowie anderen nützlichen Ressourcen bietet. Zum Öffnen der Seite klicken Sie auf das Symbol ![i für Informationen](images/info.png) in der Seitenkopfzeile.
- **Massenexport und -löschung:** Sie können jetzt gleichzeitig eine Reihe von Absichten oder Entitäten in eine CSV-Datei exportieren, um sie anschließend zu importieren und für eine andere {{site.data.keyword.conversationshort}}-Anwendung wiederzuverwenden. Sie können ebenfalls gleichzeitig eine Reihe von Entitäten oder Absichten für eine Massenlöschung auswählen.
- **Aktualisierungen für Koreanisch:** Die Tokenizer für Koreanisch wurden aktualisiert, um die Unterstützung von inoffizieller Sprache abzudecken. IBM arbeitet fortlaufend an Verbesserungen bei der Erkennung und Klassifizierung von Entitäten.
- **Emoji-Unterstützung:** Emojis, die zu Beispielen für Absichten oder als Entitätswerte hinzugefügt wurden, werden jetzt korrekt klassifiziert bzw. extrahiert. **Hinweis:** Nur Emojis, die in Ihren Trainingsdaten enthalten sind, werden korrekt und konsistent erkannt; Emojis mit abweichenden Farbtönen oder anderen Variationen werden von der Emoji-Unterstützung möglicherweise nicht korrekt klassifiziert.
- **Normalformenreduktion für Entitäten (Betaversion, nur Englisch):** Die Betaversion des Features für die unscharfe Suche legt die Stammform des Entitätswerts zu Grunde, um Entitäten zu erkennen und abzugleichen. Dieses Feature erkennt beispielsweise, dass 'bananas' Ähnlichkeit mit 'banana' hat bzw. 'run' mit 'running', da diese Wörter jeweils eine gemeinsame Stammform besitzen. Weitere Informationen finden Sie unter [Unscharfe Suche](entities.html#fuzzy-matching).
- **Verarbeitungsfortschritt beim Arbeitsbereichsimport:** Wenn Sie einen Arbeitsbereich aus einer JSON-Datei importieren, wird sofort eine Kachel für den Arbeitsbereich angezeigt, auf der Sie Informationen zum Verarbeitungsfortschritt des Imports erhalten.
- **Verringerte Trainingsdauer:** Mehrere Modelle werden jetzt parallel trainiert, was die Trainingsdauer für umfangreiche Arbeitsbereiche deutlich reduziert.

### 26. Mai 2017
{: #26May2017}

- Die aktuelle API-Version ist jetzt `2017-05-26`. Mit dieser Version werden die folgenden Änderungen eingeführt:
    - Das Schema von Objekten des Typs 'ErrorResponse' wurde geändert. Diese Änderung betrifft alle Endpunkte und Methoden. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - Das interne Schema für die Darstellung von Dialogmodulknoten in exportierten JSON-Dateien für Arbeitsbereiche wurde geändert. Falls Sie die API-Version `2017-05-26` verwenden, um einen Arbeitsbereich zu importieren, der mit einer früheren Version exportiert wurde, werden einige Knoten möglicherweise nicht korrekt importiert. Ein Arbeitsbereich sollte daher nach Möglichkeit immer mit derselben Version importiert werden, die auch für seinen Export verwendet wurde.

### 25. Mai 2017
{: #25May2017}

- Kontextvariablen können nun im Fenster 'Ausprobieren' verwaltet werden. Klicken Sie auf den Link **Kontext verwalten**, um ein neues Fenster zu öffnen, in dem Sie die Werte von Kontextvariablen direkt beim Testen des Dialogmoduls festlegen und überprüfen können. Weitere Informationen enthält der Abschnitt [Dialogmodul testen](dialog-test.html).

### 16. Mai 2017
{: #16May2017}

- Beim Öffnen des Tools ist jetzt ein Beispielarbeitsbereich **Car Dashboard** verfügbar. Sie können das Beispiel als Ausgangspunkt für Ihren eigenen Arbeitsbereich verwenden, indem Sie den Arbeitsbereich bearbeiten. Falls Sie das Beispiel für mehrere Arbeitsbereiche verwenden wollen, kopieren Sie es stattdessen. Der Beispielarbeitsbereich wird nur dann bei der Zählung für die Begrenzung der Arbeitsbereiche berücksichtigt, wenn Sie ihn verwenden.
- Die Navigation im Tool ist jetzt einfacher. Die Navigationsmenüoptionen sind jetzt am seitlichen und nicht mehr am oberen Rand der Hauptseite verfügbar. Am Anfang der Seite machen Breadcrumb-Links kenntlich, an welcher Stelle Sie sich befinden. Auf der Seite 'Arbeitsbereiche' können Sie jetzt zwischen Serviceinstanzen wechseln. Wenn Sie im Navigationsmenü auf **Zurück zu Arbeitsbereichen** klicken, können Sie diese Seite direkt aufrufen. Falls mehrere Serviceinstanzen vorhanden sind, wird der Name der aktuellen Instanz angezeigt. Sie können auf den Link **Ändern** neben der Instanz klicken, um eine andere Instanz auszuwählen.
- Wenn Sie ein Dialogmodul erstellen, werden zwei Knoten jetzt automatisch für Sie hinzugefügt: 1. Knoten **Welcome** am Anfang der Baumstruktur des Dialogmoduls, der die Begrüßung enthält, die für den Benutzer angezeigt werden soll. 2. Knoten **Anything else** am Ende der Baumstruktur für das Dialogmodul, der alle Benutzeranfragen erfasst, die von keinem anderen Knoten im Dialogmodul abgefangen werden, und diese Anfragen beantwortet. Weitere Informationen enthält der Abschnitt [Dialogmodul erstellen](dialog-create.html).
- Wenn Sie ein Dialogmodul im Fenster 'Ausprobieren' testen, können Sie nun eine kürzlich verwendete verbale Testäußerung suchen und erneut übergeben, indem Sie die Aufwärtstaste drücken und die vorherigen Eingaben durchblättern.
- Für fünf Systementitäten (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) ist jetzt die experimentelle Sprachunterstützung für Koreanisch verfügbar. Für einige der numerischen Entitäten liegen bekannte Probleme vor und die Eingabe inoffizieller Sprache wird nur begrenzt unterstützt.
- Auf der Registerkarte 'Verbessern' gibt es jetzt eine Seite 'Übersicht'. Die Seite bietet eine Zusammenfassung der Interaktionen mit Ihrem Bot. Sie können den Umfang des Datenverkehrs in einem bestimmten Zeitraum ebenso anzeigen wie die Absichten und Entitäten, die am häufigsten in Benutzerdialogen erkannt wurden. Weitere Informationen finden Sie unter [Seite 'Übersicht'](logs_oview.html).

### 27. April 2017
{: #27April2017}

- Die folgenden Systementitäten sind jetzt als Betaversionsfeatures ausschließlich in Englisch verfügbar:
    - sys-location: Erkennt die Referenzierung von Standorten wie Gemeinden, Städten und Ländern in verbalen Äußerungen der Benutzer.
    - sys-person: Erkennt die Referenzierung von Vor- und Nachnamen für Personen in verbalen Äußerungen der Benutzer.

    Weitere Angaben enthalten die [Referenzinformationen zu Systementitäten](system-entities.html).
- Die unscharfe Suche nach Entitäten ist ein Betaversionsfeature, das jetzt in Englisch verfügbar ist. Durch eine Aktivierung der unscharfen Suche für eine jeweilige Entität können Sie die Fähigkeit des Service verbessern, Benutzereingabeterms mit einer Syntax zu erkennen, die Ähnlichkeit mit der Entität hat, ohne dass eine exakte Übereinstimmung erforderlich ist. Das Feature kann die Benutzereingabe der entsprechenden Entität trotz einer falschen Schreibweise oder leichten syntaktischen Abweichungen zuordnen. Wenn Sie beispielsweise **giraffe** als Synonym einer Entität für ein Tier definieren und die Benutzereingabe den Begriff *giraffes* oder *girafe* enthält, kann die unscharfe Suche den Begriff korrekt zur Entität für das Tier zuordnen. Weitere Informationen enthält der Abschnitt [Entitäten definieren](entities.html#fuzzy-matching). Suchen Sie dort nach 'unscharfe Suche', um Details zu erfahren.

### 18. April 2017
{: #18April2017}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden Ressourcen:
    - Entitäten
    - Entitätswerte
    - Synonyme für Entitätswerte
    - Protokolle

     Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - Beim Verhalten der Methode `POST` für '/messages' wurde die Behandlung von Entitäten und Absichten geändert, die als Teil der Nachrichteneingabe angegeben sind:
    - Falls Sie Absichten in der Eingabe angeben, verwendet der Service die angegebenen Absichten, jedoch die Verarbeitung natürlicher Sprache, um Entitäten in der Benutzereingabe zu erkennen.
    - Falls Sie Entitäten in der Eingabe angeben, verwendet der Service die angegebenen Entitäten, jedoch die Verarbeitung natürlicher Sprache, um Absichten in der Benutzereingabe zu erkennen.

        Für Nachrichten, die sowohl Absichten als auch Entitäten angeben oder die weder Absichten noch Entitäten angeben, wurde das Verhalten nicht geändert.
- Die Option, eine Benutzereingabe als irrelevant zu markieren, ist jetzt für alle unterstützten Sprachen verfügbar. Dieses Feature liegt als Betaversion vor.
- Die neue Registerkarte 'Berechtigungsnachweise' bietet eine zentrale Stelle für alle Informationen, die Sie benötigen, um Ihre Anwendung mit einem Arbeitsbereich zu verbinden (z. B. die Serviceberechtigungsnachweise und die Arbeitsbereichs-ID), sowie weitere Optionen für die Bereitstellung. Klicken Sie zum Zugreifen auf die Registerkarte 'Berechtigungsnachweise' für Ihren Arbeitsbereich auf das Symbol ![Menü](images/Menu_16.png) und wählen Sie **Berechtigungsnachweise** aus.

### 9. März 2017
{: #9March2017}

Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden Ressourcen:

- Arbeitsbereiche
- Absichten
- Beispiele
- Gegenbeispiele

Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7. März 2017
{: #7March2017}

- Die Verwendung von `.` oder `..` als Name einer Absicht verursacht Probleme und wird nicht mehr unterstützt.

    Eine Absicht mit diesem Namen kann weder umbenannt noch gelöscht werden; zur Änderung des Namens müssen Sie Ihre Absichten in eine Datei importieren, die Absicht in der Datei umbenennen und die aktualisierte Datei in Ihren Arbeitsbereich importieren.

    Zahlungspflichtige Kunden können Unterstützung für eine Datenbankänderung anfordern.

### 1. März 2017
{: #1March2017}

- Systementitäten sind jetzt in Deutsch verfügbar.

### 22. Februar 2017
{: #22February2017}

- Nachrichten sind künftig auf 2048 Zeichen begrenzt.

### 3. Februar 2017
{: #3February2017}

- In diesem Release wurde die Bewertung von Absichten geändert und die Möglichkeit eingeführt, Eingabe als irrelevant für die Anwendung zu markieren. Details enthält der Abschnitt [Absichten definieren](intents.html#mark-irrelevant). Suchen Sie dort nach 'Als irrelevant markieren'.
    - Diese Features sind im Tool verfügbar, wenn Sie ein [Upgrade für Ihren Arbeitsbereich durchführen](upgrading.html).
    - Diese Features sind für Ihre Anwendung verfügbar, wenn Sie den Nachrichten-API-Aufruf so ändern, dass die Version **2017-02-03** verwendet wird.
- Die Verarbeitung von Aktionen **Springen zu** wurde geändert, um Schleifen zu verhindern, die unter bestimmten Bedingungen auftreten können.

    Details finden Sie im Abschnitt [Aktion 'Springen zu' konfigurieren](dialog-build.html#jump-to).

### 11. Januar 2017
{: #11January2017}

- In diesem Release können Sie jetzt Knotentitel im Dialogmodul anpassen.

### 22. Dezember 2016
{: #22December2016}

- In diesem Release wird in Dialogmodulknoten nun ein neuer Abschnitt für den `Knotentitel` angezeigt. Die Anpassung eines `Knotentitels` ist nicht möglich. Bei einem komprimierten Knoten wird als `Knotentitel` die `Knotenbedingung` des Dialogmodulknotens angezeigt. Falls es keine `Knotenbedingung` gibt, wird 'Nicht benannter Knoten' als Titel angezeigt.

### 19. Dezember 2016
{: #19December2016}

Die Verwendung des Dialogmodulbuilders ist jetzt durch mehrere Änderungen einfacher und intuitiver:

- Durch eine größere Bearbeitungsansicht können Sie leichter alle Details eines Knotens einsehen, wenn Sie ihn bearbeiten.
- Ein Knoten kann mehrere Antworten enthalten, die jeweils durch eine eigene Bedingung ausgelöst werden. Weitere Informationen finden Sie unter [Mehrere Antworten](dialog-build.html#responses).

### 5. Dezember 2016
{: #5December2016}

- Neue Sprachen werden unterstützt (sämtlich im Modus 'Experimentell'): Deutsch, traditionelles Chinesisch, vereinfachtes Chinesisch und Niederländisch.
- Mit '@sys-date' und '@sys-time' sind zwei neue Systementitäten verfügbar. Details finden Sie unter [Systementitäten](system-entities.html).

### 21. Oktober 2016
{: #21October2016}

- Der Service '{{site.data.keyword.conversationshort}}' bietet jetzt mit Systementitäten allgemeine Entitäten, die Sie in beliebigen Anwendungsfällen verwenden können. Details enthält der Abschnitt [Entitäten definieren](entities.html). Suchen Sie dort nach 'Systementitäten aktivieren'.
- Sie können jetzt auf der Seite 'Verbessern' ein Protokoll der Dialoge mit Benutzern anzeigen. Anhand dieses Protokolls können Sie das Verhalten des Bots nachvollziehen. Details enthält der Abschnitt [Informationen zur Komponente 'Verbessern'](logs.html).
- Sie können jetzt Entitäten aus einer CSV-Datei importieren, was bei einer großen Anzahl von Entitäten hilfreich ist. Details enthält der Abschnitt [Entitäten definieren](entities.html). Suchen Sie dort nach 'Entitäten importieren'.

### 20. September 2016
{: #20September2016}

**Neue Version:** 2016-09-20

Um die Änderungen in einer neuen Version nutzen zu können, ändern Sie den Wert des Parameters `version` in das neue Datum. Falls Sie nicht für eine Aktualisierung auf diese Version bereit sind, ändern Sie das Versionsdatum nicht.

- Version **2016-09-20**: `dialog_stack` wurde aus einem Array von Zeichenfolgen in ein Array von Objekten geändert.

### 29. August 2016
{: #29August2016}

- Sie können Dialogmodulknoten von einer Verzweigung zu einer anderen Verzweigung als gleichgeordnete Elemente oder Peerknoten verschieben. Einzelangaben enthält der Abschnitt [Dialogmodul erstellen](dialog-build.html). Suchen Sie dort nach 'Dialogmodulknoten verschieben'.
- Sie können das Fenster mit dem JSON-Editor erweitern.
- Sie können Chatprotokolle für die Dialoge Ihres Bots anzeigen, um sein Verhalten besser nachzuvollziehen. Sie können nach Absichten, Entitäten, Datum und Uhrzeit filtern. Details enthält der Abschnitt [Informationen zur Komponente 'Verbessern'](logs.html).

### 11. Juli 2016
{: #21July2016}

- Dieses allgemein verfügbare Release ermöglicht Ihnen die Arbeit mit Entitäten und Dialogmodulen und somit die Erstellung eines voll funktionsfähigen Bots.

### 18. Mai 2016
{: #18May2016}

- Dieses experimentelle Release von {{site.data.keyword.conversationshort}} führt die Benutzerschnittstelle ein und ermöglicht Ihnen die Arbeit mit Arbeitsbereichen, Absichten und Beispielen.
