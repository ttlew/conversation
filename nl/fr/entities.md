---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-16"

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

# Définition d'entités

Les ***entités*** représentent une classe d'objet ou un type de données pertinent pour l'objectif d'un utilisateur. En reconnaissant les entités qui sont mentionnées dans l'entrée utilisateur, le service {{site.data.keyword.conversationshort}} peut choisir les actions spécifiques à exécuter pour satisfaire une intention. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oSNF-QCbuDc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Nombre limite d'entités
{: #entity-limits}

Le nombre d'entités, de valeurs d'entité et de synonymes que vous pouvez créez dépend de votre plan de service {{site.data.keyword.conversationshort}} :

| Plan de service   | Nombre d'entités par espace de travail| Nombre de valeurs d'entité par espace de travail| Nombre de synonymes d'entité par espace de travail|
|-------------------|-----------------------:|----------------------------:|--------------------------------:|
| Standard/Premium  |                          1000 |                            100 000 |                              100 000 |
| Lite              |                            25 |                            100 000 |                              100 000 |

Les entités de système que vous activez pour utilisation sont prises en compte dans le nombre total d'entités utilisées de votre plan. 

## Création d'entités
{: #creating-entities}

Utilisez l'outil {{site.data.keyword.conversationshort}} pour créer des entités. 

1.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre espace de travail et cliquez sur l'onglet **Entities**. Si l'onglet **Entities** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.

1.  Cliquez sur **Create new**.

    Vous pouvez également cliquer sur **Use System Entities** pour effectuer une sélection dans une liste d'entités courantes, fournie par {{site.data.keyword.IBM_notm}}, qui s'applique à n'importe quel scénario d'utilisation. Pour plus d'informations, reportez-vous à la rubrique [Activation des entités de système](#enable_system_entities).
1.  Dans la zone **Add the entity name**, tapez un nom descriptif pour l'entité. 

    Le nom d'entité peut contenir des lettres (au format Unicode), des nombres, des traits de soulignement et des traits d'union. Par exemple :
    - `@location`
    - `@menu_item`
    - `@product`

    N'ajoutez pas le caractère `@` dans les noms d'entité lorsque vous les créez dans l'outil {{site.data.keyword.conversationshort}}. Les noms d'entité ne peuvent pas contenir d'espaces ou excéder 64 caractères. De plus, les noms d'entité ne peuvent pas débuter par la chaîne `sys-`, réservée aux entités de système.
    {: tip}

1.  Pour la fonction **Fuzzy Matching**, cliquez sur le bouton afin de sélectionner l'option On ou Off ; par défaut, cette fonction est désactivée (Off). Cette fonction est disponible pour les langues mentionnées dans la rubrique [Langues prises en charge](lang-support.html).
 {: #fuzzy-matching}

    Vous pouvez activer la fonction Fuzzy Matching (correspondance floue) afin d'améliorer la capacité du service à reconnaître les termes dans les entrées utilisateur dont la syntaxe est similaire à l'entité, mais pas forcément identique. La fonction Fuzzy Matching comporte trois options : Stemming (réduction au radical), Misspelling (fautes de frappe) et Partial Matching (correspondance partielle) : 
    - *Stemming* : avec cette option, la fonction reconnaît la forme réduite au radical de valeurs d'entité qui ont plusieurs formes grammaticales. Par exemple, la réduction au radical de 'bananas' serait 'banana', tandis que la réduction au radical de 'running' serait 'run'.
    - *Misspelling* : avec cette option, la fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez "giraffe" comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes "giraffes" ou "girafe", la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal. 
    - *Partial Matching* : avec cette option, la fonction suggère automatiquement des synonymes de type sous-chaîne présents dans les entités définies par l'utilisateur et affecte une cote de confiance inférieure par rapport à la correspondance d'entité exacte. 

    **Remarque** : pour l'anglais, la fonction Fuzzy Matching empêche la capture de certains mots anglais valides courants comme correspondances floues pour une entité donnée. Actuellement, cette fonction utilise uniquement les mots standard du dictionnaire anglais, ce qui exclut les synonymes définis par l'utilisateur.
1.  Dans la zone **Value**, tapez le texte d'une valeur possible pour l'entité. Une valeur d'entité peut être n'importe quelle chaîne de 64 caractères au maximum. 

    > **Important :** n'ajoutez pas d'informations sensibles ou personnelles dans les noms ou les valeurs d'entité. En effet, les noms et les valeurs peuvent être exposés dans les URL d'une application. 

    Une fois que vous avez entré une valeur d'entité, vous pouvez ajouter n'importe quel synonyme ou définir des canevas spécifiques pour cette valeur d'entité en sélectionnant `Synonyms` ou `Patterns` dans le menu déroulant *Type*. 

    ![Sélecteur de type pour une valeur](images/value_type.png)

    > **Remarque :** vous pouvez ajouter des synonymes *ou* des canevas pour une valeur d'entité, mais pas les deux. 

1.  Dans la zone **Synonyms**, tapez n'importe quel synonyme pour la valeur d'entité. Un synonyme peut être n'importe quelle chaîne de 64 caractères au maximum. 

    ![Capture d'écran illustrant la définition d'une entité](images/define_entity.png)
1.  La zone **Patterns** vous permet de définir des canevas spécifiques pour une valeur d'entité. Un canevas **doit** être entré sous la forme d'une expression régulière dans la zone.
  {: #pattern-entities}

    ![Capture d'écran illustrant la définition d'une entité de canevas](images/patternents1.png) 

    Dans cet exemple, pour l'entité "ContactInfo", les canevas pour les valeurs phone, email et website peuvent être définis comme suit :
    - Phone
      - `localPhone` : `(\d{3})-(\d{4})`, par exemple, 426-4968
      - `fullUSphone` : `(\d{3})-(\d{3})-(\d{4})`, par exemple, 800-426-4968
      - `internationalPhone` : `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, par exemple, +44 1962 815000
    - `email` : `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`, par exemple, name@ibm.com
    - `website` : `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, par exemple, https://www.ibm.com

    Souvent, avec des entités de canevas, il est nécessaire de stocker le texte correspondant au canevas dans une variable contextuelle (ou une variable d'action) à partir d'une arborescence de dialogue. 

    Imaginez que vous demandiez à un utilisateur son adresse électronique. La condition de noeud de dialogue contiendra une condition semblable à `@contactInfo.email`. Pour pouvoir affecter comme variable contextuelle l'adresse électronique saisie par l'utilisateur, vous pouvez utiliser la syntaxe suivante afin de capturer la correspondance de canevas dans la section de réponse du noeud de dialogue : 

    ```
    {
        "context" : {
            "email": "@contactInfo.literal"
        }
    }
    ```
    {: screen}

    Le moteur de correspondance de canevas utilisé par le service {{site.data.keyword.conversationshort}} présente des limitations en matière de syntaxe, nécessaires pour éviter tout problème de performance susceptible de se produire lors de l'utilisation d'autres moteurs d'expression régulière. En particulier, les canevas d'entité ne peuvent pas contenir :
      - Des répétitions positives (par exemple, `x*+`)
      - Des références arrières (par exemple, `\g1`)
      - Des branches conditionnelles (par exemple, `(?(cond)true))`

    Le moteur d'expression régulière est librement inspiré du moteur d'expression régulière Java. Le service {{site.data.keyword.conversationshort}} produira une erreur si vous essayez de télécharger un canevas non pris en charge, que ce soit via l'API ou à partir de l'interface utilisateur d'outils du service {{site.data.keyword.conversationshort}}. 

1.  Cliquez sur **+** et répétez la procédure pour ajouter d'autres valeurs d'entité. 
1.  Lorsque vous avez terminé d'ajouter des valeurs d'entité, cliquez sur **Done**.

### Résultats

L'entité que vous avez créée est ajoutée à l'onglet **Entities** et le système commence à s'entraîner lui-même en utilisant les nouvelles données. 

## Edition d'entités

Vous pouvez cliquer sur n'importe quelle entité de la liste pour l'ouvrir et l'éditer. Vous pouvez renommer ou supprimer des entités, et vous pouvez ajouter, éditer ou supprimer des valeurs, des synonymes ou des canevas. 

## Importation d'entités

Si vous possédez un grand nombre d'entités, vous trouverez peut-être plus facile de les importer à partir d'un fichier CSV que de les définir une par une dans l'outil {{site.data.keyword.conversationshort}}. 

**Remarque :** les canevas ne sont actuellement pas pris en charge pour l'importation d'un fichier CSV. 

1.  Collectez les entités dans un fichier CSV ou exportez-les à partir d'une feuille de calcul dans un fichier CSV. Le format requis pour chaque ligne du fichier est le suivant :

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    où &lt;entity&gt; est le nom d'une entité, &lt;value&gt; est une valeur de l'entité et &lt;synonyms&gt; est une liste de synonymes séparés par des virgules pour cette valeur. 

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    Sauvegardez le fichier CSV au format UTF-8 et sans marque d'ordre d'octet. Le fichier CSV ne doit pas excéder 10 Mo. Si votre fichier CSV est plus gros, songez à le fractionner en plusieurs fichiers et à les importer séparément. Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre espace de travail, puis cliquez sur l'onglet **Entities**.
    {: tip}
1.  Cliquez sur ![Import](images/importGA.png), puis faites glisser un fichier, ou recherchez un fichier sur votre ordinateur. Le fichier est validé et importé et le système commence à s'entraîner lui-même en utilisant les nouvelles données. 

### Résultats

Vous pouvez visualiser les entités importées sur l'onglet Entities. Vous devrez peut-être actualiser la page pour voir les nouvelles entités. 

## Exportation d'entités
{: #export_entities}

Vous pouvez exporter un certain nombre d'entités vers un fichier CSV, de manière à pouvoir les importer et les réutiliser pour une autre application Conversation.

**Remarque :** les canevas ne sont actuellement pas pris en charge pour l'exportation d'un fichier CSV. 

1.  Sur l'onglet Entities, sélectionnez l'icône ![Export](images/ExportIcon.png)

    ![Options Export et Delete](images/ExportEntity1.png)

1.  Sélectionnez les entités souhaitées, puis cliquez sur le bouton **Export**. 

    ![Sélection d'entité pour les boutons Export et Delete](images/ExportEntity2.png)

## Suppression d'entités
{: #delete_entities}

Vous pouvez sélectionner un certain nombre d'entités à supprimer. 

**Important** : lorsque vous supprimez des entités, vous supprimez également toutes les valeurs, tous les synonymes ou tous les canevas qui leur sont associés, et ces éléments ne peuvent plus être extraits ultérieurement. Tous les noeuds de dialogue qui font référence à ces entités ou ces valeurs doivent être mis à jour manuellement de manière à ne plus faire référence au contenu supprimé. 

1.  Sur l'onglet Entities, sélectionnez l'icône ![Delete](images/DeleteIcon.png)

    ![Options Export et Delete](images/DeleteEntity.png)

1.  Sélectionnez les entités que vous souhaitez supprimer, puis cliquez sur le bouton **Delete**. **Remarque** : la fonction de suppression prend en charge la suppression en bloc de plusieurs entités. 

## Activation des entités de système
{: #enable_system_entities}

Le service {{site.data.keyword.conversationshort}} fournit un certain nombre d'*entités de système* ; il s'agit d'entités communes que vous pouvez utiliser pour n'importe quelle application. Le fait d'activer une entité de système permet de rendre possible le remplissage rapide de votre espace de travail avec des données d'apprentissage communes à un grand nombre de scénarios d'utilisation. 

Les entités de système peuvent être utilisées pour reconnaître un large éventail de valeurs pour les types d'objet qu'elles représentent. Par exemple, l'entité de système `@sys-number` correspond à n'importe quelle valeur numérique, y compris des nombres entiers, des fractions décimales ou même des nombres écrits sous forme de mots. 

Les entités de système sont gérées de manière centralisée, par conséquent, toutes les mises à jour sont disponibles automatiquement. Vous ne pouvez pas modifier les entités de système.

1.  Sur l'onglet Entities, cliquez sur **System entities**.

    ![Capture d'écran de l'onglet "System entities"](images/system_entities_1.png)

    Si vous n'avez pas encore créé d'entités, cliquez sur **Use system entities**.

    ![Capture d'écran du bouton "Use system entities"](images/system_entities_2.png)

1.  Parcourez la liste des entités de système afin de choisir celles qui sont utiles pour votre application.
    - Pour afficher davantage d'informations sur une entité de système, y compris des exemples d'entrée correspondante, cliquez sur l'entité dans la liste. 
    - Pour obtenir des informations complètes sur les entités de système disponibles, reportez-vous à la rubrique [Entités de système](system-entities.html).
1.  Cliquez sur le bouton bascule en regard d'une entité de système pour l'activer ou la désactiver. Vous pouvez également activer ou désactiver toutes les entités de système en cliquant sur le bouton **All toggle** en haut de la liste. 

### Résultats

Une fois que vous avez activé les entités de système, le service {{site.data.keyword.conversationshort}} recommence à s'entraîner. Une fois l'entraînement terminé, vous pouvez utiliser les entités. 
