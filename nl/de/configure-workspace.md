---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-17"

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

# Conversation-Arbeitsbereich konfigurieren

Die Verarbeitung natürlicher Sprache für den Service '{{site.data.keyword.conversationshort}}' findet in einem *Arbeitsbereich* statt. Ein Arbeitsbereich ist ein Container für alle Artefakte, die den Dialogablauf für eine Anwendung definieren.
{: shortdesc}

Eine {{site.data.keyword.conversationshort}}-Serviceinstanz kann mehrere Arbeitsbereiche umfassen. Ein Arbeitsbereich enthält die folgenden Typen von Artefakten:

- [**Absichten**](intents.html): Eine *Absicht* bildet den Zweck einer Benutzereingabe ab, z. B. die Frage nach Filialstandorten oder einer Rechnungszahlung. Sie definieren eine Absicht für jeden Typ einer Benutzeranforderung, den Ihre Anwendung unterstützen soll. Im Tool wird für den Namen einer Absicht immer das Zeichen `#` als Präfix verwendet. Um den Arbeitsbereich für die Erkennung Ihrer Absichten zu trainieren, stellen Sie viele Beispiele für Benutzereingaben bereit und geben an, welchen Absichten sie zugeordnet sind.
- [**Entitäten**](entities.html): Eine *Entität* stellt einen Term oder ein Objekt dar, der/das für die Absichten relevant ist und einen speziellen Kontext für die Absicht bereitstellt. Eine Entität kann beispielsweise eine Stadt sein, in der ein Benutzer einen Filialstandort sucht, oder der Betrag einer Rechnungszahlung. Im Tool wird für den Namen einer Entität immer das Zeichen `@` als Präfix verwendet. Um den Arbeitsbereich für die Erkennung Ihrer Entitäten zu trainieren, listen Sie die möglichen Werte für jede Entität sowie Synonyme auf, die der Benutzer eingeben könnte.
- [**Dialogmodul**](dialog-build.html): Ein *Dialogmodul* ist ein Dialogablauf mit Verzweigungen, der definiert, wie Ihre Anwendung antwortet, wenn die definierten Absichten und Entitäten erkannt werden. Mit dem tooleigenen Dialogmodulbuilder können Sie Dialoge mit Benutzern erstellen und hierbei Antworten angeben, die auf den in der Benutzereingabe erkannten Absichten und Entitäten basieren.

Sobald Sie Informationen hinzufügen, trainiert sich der Arbeitsbereich selbst. Sie müssen keine Aktion ausführen, um das Training auszulösen.

## Begrenzung für Arbeitsbereiche
{: #workspace-limits}

Die Anzahl der Arbeitsbereiche, die Sie in einer einzelnen Serviceinstanz erstellen können, richtet sich nach Ihrem {{site.data.keyword.conversationshort}}-Serviceplan. Der Beispielarbeitsbereich wird nur dann bei der Zählung für die Begrenzung der Arbeitsbereiche berücksichtigt, wenn Sie das Beispiel bearbeiten oder kopieren:

| Serviceplan      | Arbeitsbereiche pro Instanz     |
|------------------|--------------------------------:|
| Standard/Premium |                              20 |
| Lite*            |                               5 |

*Nach 30 Tagen Inaktivität kann ein nicht verwendeter Lite-Arbeitsbereich gelöscht werden, um Speicherplatz freizugeben.

## Arbeitsbereiche erstellen
{: #creating-workspaces}

Sie können einen Arbeitsbereich völlig neu erstellen, den bereitgestellten Beispielarbeitsbereich verwenden oder einen Arbeitsbereich aus einer JSON-Datei importieren. Sie können auch einen vorhandenen Arbeitsbereich in derselben Serviceinstanz duplizieren.

Zur Erstellung von Arbeitsbereichen verwenden Sie das {{site.data.keyword.conversationshort}}-Tool. So erstellen Sie einen Arbeitsbereich:

1.  Wenn die Seite 'Servicedetails' noch nicht geöffnet ist, klicken Sie in der Konsole auf Ihre {{site.data.keyword.conversationshort}}-Serviceinstanz. (Wenn Sie eine Serviceinstanz erstellen, wird die Seite 'Servicedetails' angezeigt.)

1.  Blättern Sie auf der Seite 'Servicedetails' bis **Dialogtools** vor und klicken Sie auf **Starttool**.

1.  Führen Sie eine der folgenden Aktionen aus:
    - Klicken Sie auf **Erstellen**, wenn Sie einen Arbeitsbereich völlig neu erstellen wollen.
    - Bearbeiten Sie den Beispielarbeitsbereich, wenn Sie das Beispiel als Ausgangspunkt für Ihren eigenen Arbeitsbereich verwenden wollen. Falls Sie das Beispiel für mehrere Arbeitsbereiche verwenden wollen, duplizieren Sie es stattdessen.
    - Klicken Sie auf das Symbol ![Arbeitsbereich importieren](images/workspace_import.png) und wählen Sie die JSON-Datei für den gewünschten Import aus, wenn Sie einen Arbeitsbereich aus einer JSON-Datei importieren wollen.

        **Wichtig:**

        - Die importierte JSON-Datei muss die UTF-8-Codierung verwenden.
        - Die maximale Größe einer JSON-Datei für einen Arbeitsbereich beträgt 10 MB. Falls Sie einen größeren Arbeitsbereich importieren müssen, kann es sinnvoll sein, die Absichten und Entitäten gesondert zu importieren, nachdem Sie den Arbeitsbereich importiert haben. (Für den Import von größeren Arbeitsbereichen können Sie auch die REST-API nutzen. Weitere Informationen finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
        - Die JSON-Datei darf keine Tabstopps, Zeilenumbrüche oder Wagenrückläufe enthalten.

        Geben Sie die Daten an, die Sie einbeziehen wollen:

        - Wählen Sie **Alles (Absichten, Entitäten und Dialogmodul)** aus, falls Sie eine vollständige Kopie des exportierten Arbeitsbereichs inklusive des Dialogmoduls importieren wollen.
        - Wählen Sie **Absichten und Entitäten** aus, falls Sie die Absichten und Entitäten aus dem exportierten Arbeitsbereich verwenden, jedoch ein neues Dialogmodul erstellen wollen. 

1.  Geben Sie die Details für den neuen Arbeitsbereich an:
    - **Name**: Der Name darf nicht länger als 64 Zeichen sein. Dieser Wert ist erforderlich.
    - **Beschreibung**: Die Beschreibung darf nicht länger als 128 Zeichen sein.
    - **Sprache**: Die Sprache der Benutzereingabe, für deren Verständnis der Arbeitsbereich trainiert wird.

Nachdem Sie den Arbeitsbereich erstellt haben, wird er auf der Seite 'Arbeitsbereiche' als Kachel angezeigt.

## Arbeitsbereiche Symbol exportieren und kopieren
{: #exporting-and-copying-workspaces}

Auf der Seite 'Arbeitsbereiche' können Sie Arbeitsbereiche exportieren und einen Arbeitsbereich in einen neuen Arbeitsbereich kopieren.

Beim Exportieren eines Arbeitsbereichs wird eine Kopie aller Arbeitsbereichsdaten in einer JSON-Datei gespeichert. Verwenden Sie diese Option, wenn Sie den Arbeitsbereich in eine andere Serviceinstanz importieren oder offline eine Kopie des Arbeitsbereichs speichern wollen. Beim Importieren eines Arbeitsbereichs können Sie auswählen, dass lediglich die Absichten und Entitäten importiert werden sollen. Dies kann von Nutzen sein, wenn Sie ein neues Dialogmodul unter Verwendung derselben Trainingsdaten erstellen wollen.

Beim Kopieren eines Arbeitsbereichs wird eine vollständige Kopie des Arbeitsbereichs innerhalb derselben Serviceinstanz erstellt. Verwenden Sie diese Option, wenn Sie einen bestehenden Arbeitsbereich anpassen, die ursprüngliche Version jedoch ebenfalls beibehalten wollen.

- Zum Exportieren eines bestehenden Arbeitsbereichs in eine JSON-Datei klicken Sie auf der Kachel für den Arbeitsbereich auf das Menüsymbol und wählen Sie dann die Option **Als JSON herunterladen** aus:

    ![Screenshot mit der Menüoption 'Als JSON herunterladen'](images/workspace_export.png)
- Um eine Kopie eines bestehenden Arbeitsbereichs innerhalb derselben Serviceinstanz zu erstellen, klicken Sie auf der Kachel für den Arbeitsbereich auf das Menüsymbol und wählen Sie dann die Option **Duplizieren** aus:

    ![Screenshot mit der Menüoption 'Duplizieren'](images/workspace_duplicate.png)

    Geben Sie den Namen, die Beschreibung und die Sprache für den neuen Arbeitsbereich an. In der Kopie sind alle Daten (inklusive Absichten, Entitäten und Dialogmodul) enthalten.

