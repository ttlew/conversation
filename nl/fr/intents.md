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

# Définition d'intentions

Les ***intentions*** sont des objectifs exprimés dans une entrée d'un client, par exemple, répondre à une question ou régler une facture. En reconnaissant l'intention exprimée dans une entrée d'un client, le service {{site.data.keyword.conversationshort}} peut choisir le flux de dialogue approprié pour y répondre.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VG0YykNcfv8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Nombre limite d'intentions
{: #intent-limits}

Le nombre d'intentions et d'exemples que vous pouvez créez dépend de votre plan de service {{site.data.keyword.conversationshort}} :

| Plan de service  | Nombre d'intentions par espace de travail| Nombre d'exemples par espace de travail |
|------------------|----------------------:|-----------------------:|
| Standard/Premium |                 2 000 |                 25 000 |
| Lite             |                    25 |                 25 000 |

## Création d'intentions
{: #creating-intents}

Utilisez l'outil {{site.data.keyword.conversationshort}} pour créer des intentions. 

1.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre espace de travail, puis sélectionnez l'onglet **Intents** dans la barre de navigation.
    Si l'onglet **Intents** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.

1.  Sélectionnez **Create new**.
1.  Dans la zone **Intent name**, tapez un nom descriptif pour l'intention. 
    - Le nom d'intention peut contenir des lettres (au format Unicode), des nombres, des traits de soulignement, des traits d'union et des points. 
    - Le nom ne peut pas correspondre à `..` ni à aucune autre chaîne composée uniquement de points. 
    - Les noms d'intention ne peuvent pas contenir d'espaces et ne doivent pas excéder 128 caractères. Voici quelques exemples de noms d'intention :
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    Les outils incluent automatiquement le caractère `#` dans les noms d'intention, par conséquent, vous n'avez pas besoin d'en ajouter un.
    {: tip}

    Vous pouvez sélectionner **Create** pour sauvegarder votre nom d'intention sans ajouter d'exemples. Vous pouvez également sélectionner la zone User example, ou utiliser la touche de tabulation pour vous déplacer vers l'avant, puis ajouter des exemples. 

1.  Dans la zone **User example**, tapez le texte d'un exemple d'utilisateur pour l'intention. Un exemple peut être n'importe quelle chaîne de 1024 caractères au maximum. Voici quelques exemples d'intention `#pay_bill` :
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    Si vous avez défini ou prévu de définir des entités qui correspondent à cette intention, reportez-vous aux entités ou aux synonymes qui leur sont associés dans certains des exemples. Cela vous permettra d'établir une relation entre l'intention et les entités.

    > **Important :** les noms d'intention et l'exemple de texte peuvent être exposés dans des URL lorsqu'une application interagit avec le service. N'ajoutez pas d'informations sensibles ou personnelles dans ces artefacts. 

    Appuyez sur Entrée ou sélectionnez **+** pour sauvegarder l'exemple.
1.  Répétez le même processus pour ajouter d'autres exemples. Vous pouvez passer d'un exemple à l'autre à l'aide de la touche de tabulation. Indiquez au moins 5 exemples pour chaque intention. Plus vous fournirez d'exemples, plus votre application sera précise. 

    ![Capture d'écran illustrant la définition d'une intention](images/define_intent.png)
1.  Lorsque vous avez fini d'ajouter des exemples, sélectionnez **Done** pour terminer la création de l'intention. 

### Résultats

L'intention que vous avez créée est ajoutée à l'onglet Intents et le système commence à s'entraîner lui-même en utilisant les nouvelles données. 

## Edition d'intentions

Vous pouvez sélectionner n'importe quelle intention de la liste pour l'ouvrir et l'éditer. Vous pouvez effectuer les modifications suivantes :

- Renommer l'intention
- Supprimer l'intention
- Ajouter, éditer, ou supprimer des exemples
- Déplacer un exemple vers une autre intention

Vous pouvez passer du nom d'intention à chaque exemple à l'aide de touche de tabulation, et le cas échéant, éditer les exemples. 

Pour déplacer un exemple, sélectionnez-le en cochant la case qui lui correspond, puis sélectionnez **Move to**.

  ![Capture d'écran illustrant la procédure de déplacement d'un exemple](images/move_example.png)

## Importation d'intentions et d'exemples

Si vous possédez un grand nombre d'intentions et d'exemples, vous trouverez peut-être plus facile de les importer à partir d'un fichier CSV que de les définir un par un dans l'outil {{site.data.keyword.conversationshort}}. 

1.  Collectez les intentions et les exemples dans un fichier CSV ou exportez-les à partir d'une feuille de calcul dans un fichier CSV. Le format requis pour chaque ligne du fichier est le suivant :

    ```
    <example>,<intent>
    ```
    {: screen}

    où `<example>` est le texte de l'exemple d'utilisateur, et `<intent>` est le nom de l'intention à laquelle l'exemple doit correspondre. Par exemple :

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    > **Important :** sauvegardez le fichier CSV au format UTF-8 et sans marque d'ordre d'octet. 

1.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre espace de travail, puis sélectionnez l'onglet **Intents** dans la barre de navigation.
    Si l'onglet **Intents** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.


1.  Sélectionnez ![Import](images/importGA.png), puis faites glisser un fichier, ou recherchez un fichier sur votre ordinateur. Le fichier est validé et importé et le système commence à s'entraîner lui-même en utilisant les nouvelles données. 

    > **Important :** le fichier CSV ne doit pas excéder 10 Mo. Si votre fichier CSV est plus gros, songez à le fractionner en plusieurs fichiers et à les importer séparément. 

### Résultats

Vous pouvez visualiser les intentions importées et les exemples correspondants sur l'onglet Intents. Vous devrez peut-être actualiser la page pour voir les nouvelles entités et les nouveaux exemples. 

## Exportation d'intentions
{: #export_intents}

Vous pouvez exporter un certain nombre d'intentions vers un fichier CSV, de manière à pouvoir les importer et les réutiliser pour une autre application Conversation.

1.  Sur l'onglet Intents, sélectionnez l'icône ![Export](images/ExportIcon.png)

    ![Options Export et Delete](images/ExportIntent1.png)

1.  Sélectionnez les intentions souhaitées, puis cliquez sur le bouton **Export**.
    ![Sélection d'intention pour les boutons Export et Delete](images/ExportIntent2.png)

## Suppression d'intentions
{: #delete_intents}

Vous pouvez sélectionner un certain nombre d'intentions à supprimer. 

**Important** : lorsque vous supprimez des intentions, vous supprimez également tous les exemples qui leur sont associés, et ces éléments ne peuvent plus être extraits ultérieurement. Tous les noeuds de dialogue qui font référence à ces intentions doivent être mis à jour manuellement de manière à ne plus faire référence au contenu supprimé. 

1.  Sur l'onglet Intents, sélectionnez l'icône ![Delete](images/DeleteIcon.png)

    ![Options Export et Delete](images/ExportIntent3.png)

1.  Sélectionnez les intentions que vous souhaitez supprimer, puis cliquez sur le bouton **Delete**. **Remarque** : la fonction de suppression prend en charge la suppression en bloc de plusieurs intentions. 

## Test de vos intentions
{: #testing-your-intents}

Lorsque vous avez fini de créer de nouvelles intentions, vous pouvez tester le système pour voir s'il reconnaît vos intentions comme prévu. 

1.  Dans l'outil {{site.data.keyword.conversationshort}}, sélectionnez l'icône ![Demander à Watson](images/ask_watson.png). 

1.  Sur le panneau Try it out, tapez une question ou une autre chaîne de texte et appuyez sur Entrée pour voir quelle intention est reconnue. Si l'intention reconnue n'est pas celle qui est prévue, vous pouvez améliorer votre modèle en ajoutant ce texte comme exemple dans l'intention appropriée. 

    Si vous avez récemment modifié votre espace de travail, il est possible qu'un message indiquant que le système est toujours en phase d'entraînement s'affiche. Lorsque cela se produit, attendez la fin de la phase d'entraînement avant de lancer les tests :
    {: tip}

    ![Capture d'écran illustrant le message qui s'affiche lorsque Watson est en cours d'entraînement](images/training.png)

    La réponse indique quelle intention a été reconnue à partir de votre entrée. 

    ![Capture d'écran illustrant le test des intentions](images/test_intents.png)

1.  Si le système n'a pas reconnu l'intention appropriée, vous pouvez corriger cela. Pour ce faire, sélectionnez l'intention affichée et choisissez l'intention correcte dans la liste. Une fois la correction soumise, le système reprend automatiquement son entraînement pour incorporer les nouvelles données. 

    ![Capture d'écran illustrant la correction d'une intention reconnue](images/correct_intent.png)

1.  Si l'entrée est sans rapport avec votre application, vous pouvez indiquer ce fait. Sélectionnez l'intention affichée et choisissez **Mark as irrelevant**.

    ![Capture d'écran illustrant l'option Mark as irrelevant](images/irrelevant.png)

Si vos intentions ne sont pas correctement reconnues, songez à effectuer les modifications suivantes :

- Ajoutez le texte non reconnu comme exemple dans l'intention correcte. 
- Déplacez les exemples d'une intention vers une autre.
- Déterminez si vos intentions sont trop similaires, puis redéfinissez-les le cas échéant. 

## Options Absolute scoring et Mark as irrelevant

Depuis février 2017, il existe un nouvel algorithme permettant d'évaluer la cote de confiance d'une intention et de renvoyer des intentions. Vous pouvez également marquer des entrées comme non pertinentes ("irrelevant"). Ces modifications peuvent nécessiter la [mise à niveau de votre espace de travail  ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](upgrading.html){: new_window}.

### Option Absolute scoring

A présent, nous évaluons la cote de confiance de chaque intention isolément et non par rapport à d'autres intentions. Cela offre la souplesse nécessaire pour permettre le renvoi de plusieurs intentions. Cela signifie également que le système peut ne pas renvoyer d'intention du tout. Si le système pense qu'il y a peu de chances qu'une intention soit liée à l'entrée utilisateur (cote de confiance inférieure à .2), il ne renvoie pas l'intention concernée. 

Si les cotes de confiance relatives aux intentions changent, vous pouvez être amené à restructurer vos dialogues. Par exemple, si vous avez conditionné votre dialogue avec une intention dont la cote de confiance est maintenant faible, la réponse du système ne sera plus valide. 

### Option Mark as irrelevant
{: #mark-irrelevant}

Vous pouvez vous reporter à la rubrique [Langues prises en charge](lang-support.html) pour connaître la disponibilité de cette fonction. 

Après avoir mis à niveau votre espace de travail, vous pouvez [tester les entrées](#testing-your-intents) dans le panneau Try it out pour voir les modifications. Vous pouvez utiliser l'option "Mark as irrelevant" pour indiquer que l'entrée est sans rapport avec votre application. 

Si vous avez une intention, telle que #off_topic, pour les entrées qui sont hors de portée ou hors sujet, supprimez l'intention et testez votre espace de travail en marquant les entrées comme non pertinentes. 

> **Important** : les entrées marquées comme non pertinentes sont stockées dans l'espace de travail et sont incluses dans le cadre des données d'apprentissage. Vous devez être certain que vous souhaitez vraiment effectuer cette modification. 
> - Les entrées ne sont plus accessibles et ne peuvent plus être modifiées ultérieurement dans les outils.
> - Le seul moyen de retirer la balise "Irrelevant" est d'utiliser la même entrée dans le panneau Try it out, puis de modifier l'intention. 
