---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-06"

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

# Upgrade für Arbeitsbereiche durchführen

Die Features des Service 'Conversation' werden regelmäßig erweitert und aktualisiert. Einige dieser Änderungen werden zwar automatisch auf Ihren Arbeitsbereich angewendet, aber Updates mit weitreichenden Auswirkungen machen eine manuelle Aktualisierung erforderlich.
{: shortdesc}

Falls Sie eine Instanz verwenden, die vor dem 3. Februar 2017 erstellt wurde, und für Ihren Arbeitsbereich eine Aktualisierung verfügbar ist, wird das Upgradesymbol ![Upgradesymbol](images/upgrade.png) angezeigt, um Sie darauf hinzuweisen, dass ein Upgrade verfügbar ist. Wählen Sie dieses Symbol aus, um das Dialogfenster 'Upgrade für Arbeitsbereich durchführen' zu öffnen.

Sobald Sie ein Upgrade Ihres Arbeitsbereichs durchführen, wird die neueste Version der API im Tool aktiviert und in der Anzeige 'Ausprobieren' mit der Verwendung der neuesten Features begonnen.

Nachdem Sie ein Upgrade für einen Arbeitsbereich durchgeführt haben, können Sie den Arbeitsbereich nicht mehr auf eine Vorgängerversion zurücksetzen.

## Upgrade Ihres Arbeitsbereichs durchführen
Führen Sie Folgendes aus, um zu verhindern, dass Funktionalität in Ihrem Arbeitsbereich behindert wird:

1.  [Duplizieren Sie Ihren Arbeitsbereich](configure-workspace.html#exporting-and-copying-workspaces).
2.  Führen Sie das Upgrade für den duplizierten Arbeitsbereich durch.
3.  Testen Sie, ob in diesem Arbeitsbereich nach dem Upgrade Probleme auftreten.

Nachdem Sie den Test auf Probleme beendet haben, wenden Sie das Upgrade auf Ihre Anwendung an, indem Sie den Nachrichten-API-Aufruf so ändern, dass **2017-02-03** oder eine höhere Version verwendet wird.

## Änderung bei Aktionen 'Springen zu'
Die Verarbeitung von Aktionen **Springen zu** wurde im Release vom 3. Februar 2017 geändert.

Wenn Sie zuvor zur Bedingung eines Knotens gesprungen sind und weder dieser Knoten noch einer seiner Peerknoten eine Bedingung enthielten, die mit 'true' ausgewertet wurde, führte das System die Aktion 'Springen zu' zum Stammknoten aus und suchte nach einem Knoten, dessen Bedingung mit der Eingabe übereinstimmte. In einigen Situationen entstand durch diese Verarbeitung eine Schleife, die eine Fortsetzung des Dialogmoduls verhinderte.

Wenn beim neuen Prozess weder der Zielknoten noch einer seiner Peerknoten mit 'true' ausgewertet werden, wird die Dialogmodulrunde beendet. Eine etwaige Antwort, die generiert wurde, wird an den Benutzer zurückgegeben, und an die Anwendung wird eine Fehlernachricht zurückgegeben:

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

Die nächste Benutzereingabe wird auf der Stammebene des Dialogmoduls verarbeitet.

Diese Aktualisierung könnte das Verhalten Ihres Dialogmoduls ändern, wenn Sie darin Aktionen **Springen zu** verwenden, deren Ziel Knoten mit Bedingungen sind, die ebenfalls mit 'false' ausgewertet werden.

Falls Sie das alte Verarbeitungsmodell wiederherstellen wollen, fügen Sie einen finalen Peerknoten mit einer Bedingung `true` hinzu. Verwenden Sie in der Antwort eine Aktion **Springen zu**, deren Ziel die Bedingung des ersten Knotens auf der Stammebene der Baumstruktur Ihres Dialogmoduls ist.
