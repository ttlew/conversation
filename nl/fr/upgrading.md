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

# Mise à niveau des espaces de travail

Le service Conversation ajoute et met à jour des fonctions régulièrement. Si certaines modifications sont appliquées automatiquement à votre espace de travail, celles qui ont un impact majeur nécessitent d'être appliquées manuellement.
{: shortdesc}

Si vous possédez une instance qui a été créée avant le 3 février 2017 et qu'une mise à jour est disponible pour votre espace de travail, l'icône de mise à niveau (![Upgrade](images/upgrade.png)) apparaît pour indiquer qu'une mise à niveau est disponible. Sélectionnez cette icône pour ouvrir la boîte de dialogue Upgrade workspace. 

Lorsque vous mettez à niveau votre espace de travail, la dernière version de l'API est activée dans l'outil et le panneau "Try it out" commence à utiliser les fonctions les plus récentes. 

Une fois que vous avez mis à niveau un espace de travail, vous ne pouvez plus rétablir la version précédente de cet espace de travail. 

## Mise à niveau de votre espace de travail
Pour éviter d'entraver la fonctionnalité dans votre espace de travail :

1.  [Dupliquez votre espace de travail](configure-workspace.html#exporting-and-copying-workspaces).
2.  Mettez à niveau l'espace de travail dupliqué. 
3.  Testez l'espace de travail mis à niveau. 

Lorsque vous avez terminé les tests, appliquez la mise à niveau à votre application en modifiant l'appel d'API de message pour que la date **2017-02-03** ou une date ultérieure soit utilisée. 

## Modifications apportées aux actions Jump to
Le traitement des actions **Jump to** a été modifié dans le cadre de l'édition du 3 février 2017. 

Auparavant, si vous passiez directement à la condition d'un noeud et que ni ce noeud ni aucun de ses noeuds homologues n'avaient une condition pour laquelle la valeur true avait été renvoyée, le système accédait directement au noeud de niveau racine et recherchait un noeud dont la condition correspondait à l'entrée. Dans certaines situations, ce traitement créait une boucle, ce qui empêchait le dialogue de progresser. 

Dans le cadre du nouveau processus, si le noeud cible et ses homologues ne renvoient pas la valeur true, le tour du dialogue prend fin. Les réponses qui ont été éventuellement générées sont renvoyées à l'utilisateur et un message d'erreur est renvoyé à l'application : 

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

L'entrée utilisateur suivante est traitée au niveau racine du dialogue. 

Cette mise à jour peut modifier le comportement de votre dialogue si certaines actions **Jump to** ciblent des noeuds dont les conditions sont false.

Si vous souhaitez restaurer l'ancien modèle de processus, ajoutez un noeud homologue final avec une condition `true`. Dans la réponse, utilisez une action **Jump to** qui cible la condition du premier noeud au niveau racine de l'arborescence de votre dialogue. 
