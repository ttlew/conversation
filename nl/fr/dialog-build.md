---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-19"

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

# Création d'un dialogue
{: #dialog-build}

Le dialogue utilise les intentions et les entités qui sont identifiées dans l'entrée utilisateur, plus le contexte issu de l'application, pour interagir avec l'utilisateur et, in fine, fournir une réponse utile.
{: shortdesc}

La réponse peut être la réponse à une question, telle que `Où puis-je trouver de l'essence ?` ou l'exécution d'une commande, comme allumer la radio. L'intention et l'entité peuvent suffire pour identifier la réponse appropriée, ou bien le dialogue peut demander à l'utilisateur de saisir davantage d'informations pour répondre correctement. Par exemple, si un utilisateur demande "Où puis-je trouver de la nourriture ?", vous souhaiterez peut-être lui demander de préciser s'il recherche un restaurant ou un magasin d'alimentation, pour dîner sur place ou pour emporter, etc. Vous pouvez réclamer davantage d'informations dans une réponse textuelle et créer un ou plusieurs noeuds enfant pour traiter la nouvelle entrée. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oQUpejt6d84?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Présentation du dialogue
{: #overview}

Votre dialogue est représenté graphiquement dans l'outil sous la forme d'une arborescence. Créez une branche pour traiter chaque intention qui doit être gérée par votre conversation. Une branche est composée de plusieurs noeuds. 

### Noeuds de dialogue

Chaque noeud de dialogue contient au moins une condition et une réponse. 

![Graphique représentant une entrée utilisateur reliée par une flèche droite à une boîte contenant l'instruction If: CONDITION, Then: REPONSE](images/node1-empty.png)

- Condition : indique les informations que l'entrée utilisateur doit contenir pour déclencher ce noeud dans le dialogue. Ces informations peuvent être une intention spécifique, une valeur d'entité ou une valeur de variable contextuelle. Pour plus d'informations, reportez-vous à la section [Conditions](#conditions). 
- Réponse : énoncé que le service utilise pour répondre à l'utilisateur. La réponse peut aussi être configurée pour déclencher des actions par programmation. Pour plus d'informations, reportez-vous à la section [Réponses](#responses). 

Le noeud est basé sur une construction if/then : si cette condition a pour valeur true, renvoyer cette réponse. 

Par exemple, le noeud ci-dessous est déclenché si la fonction de traitement du langage naturel du service détermine que l'entrée utilisateur contient l'intention `#cupcake-menu`. Suite au déclenchement du noeud, le service fournit une réponse appropriée. 

![Graphique représentant la question posée par l'utilisateur au sujet des parfums proposés pour les cupcakes. La condition If est #cupcake-menu et la réponse Then correspond à une liste répertoriant les parfums des cupcakes.](images/node1-simple.png)

Un noeud comportant une condition et une réponse peut gérer des demandes d'utilisateur simples. Mais, la plupart du temps, les utilisateurs posent des questions plus sophistiquées ou demandent de l'aide pour des tâches plus complexes. Vous pouvez ajouter des noeuds enfant qui demandent à l'utilisateur de fournir les informations supplémentaires dont le service a besoin. 

![Graphique représentant le premier noeud du dialogue demandant à l'utilisateur s'il souhaite acheter des cupcakes ordinaires ou sans gluten. Ce noeud est associé à deux noeuds enfant qui fournissent une réponse différente selon la réponse de l'utilisateur.](images/node1-children.png)

### Flux de dialogue

Le dialogue que vous créez est traité par le service de haut en bas. 

![Une flèche vers le bas est représentée à côté de 3 noeuds pour montrer que le flux du dialogue se fait de haut en bas ](images/node-flow-down.png)

A mesure que le service se déplace vers le bas dans l'arborescence, s'il détecte une condition qui est satisfaite, il déclenche ce noeud. Il se déplace alors de la gauche vers la droite sur le noeud déclenché afin de comparer l'entrée utilisateur à d'éventuelles conditions de noeud enfant. A mesure qu'il effectue ces comparaisons avec des noeuds enfant, il se déplace de nouveau de haut en bas.

Le service continue de fonctionner à travers l'arborescence du dialogue, de haut en bas, de gauche à droite, puis de haut en bas et de gauche à droite jusqu'à ce qu'il atteigne le dernier noeud de la branche qu'il parcourt. 

![Graphique représentant une flèche portant le numéro 1 dirigée vers le bas, une flèche portant le numéro 2 dirigée vers la droite, et une flèche portant le numéro 3 dirigée vers le bas, un niveau de noeud au-dessous. ](images/node-flow.png)

Lorsque vous commencez à créer le dialogue, vous devez déterminer les branches que vous souhaitez inclure, et l'endroit où vous souhaitez les placer. L'ordre des branches est important car l'évaluation des noeuds est réalisée de haut en bas. Le premier noeud de base dont la condition correspond à l'entrée est utilisé ; les autres noeuds de niveau inférieur dans l'arborescence ne sont pas déclenchés. 

## Conditions
{: #conditions}

Une condition de noeud détermine si ce noeud est utilisé dans la conversation. Les conditions de réponse déterminent la réponse qui doit s'afficher pour un utilisateur.
Vous pouvez utiliser un ou plusieurs des artefacts suivants, dans n'importe quelle combinaison, pour définir une condition : 

- **Variable contextuelle** : le noeud est utilisé si l'expression de variable contextuelle que vous spécifiez a pour valeur true. Utilisez la syntaxe `$variable_name:value` ou `$variable_name == 'value'`. Reportez-vous à la section [Variables contextuelles](#context).

  Vous ne devez pas définir une condition de noeud ou de réponse en fonction de la valeur d'une variable contextuelle dans le même noeud de dialogue que celui dans lequel vous définissez cette valeur de variable contextuelle.
  {: tip}

- **Entité** : le noeud est utilisé dès lors qu'une valeur ou un synonyme de l'entité est reconnu dans l'entrée utilisateur. Utilisez la syntaxe `@entity_name`. Par exemple, `@city`.

  Prenez soin de créer un noeud homologue pour gérer les situations où les valeurs ou les synonynes de l'entité ne sont pas du tout reconnus.
  {: tip}

- **Valeur d'entité** : le noeud est utilisé si la valeur d'entité est détectée dans l'entrée utilisateur. Utilisez la syntaxe `@entity_name:value`. Par exemple, `@city:Boston`. Spécifiez une valeur définie pour l'entité, et non un synonyme.

  Si vous recherchez l'entité, sans spécifier de valeur spécifique pour cette dernière, dans un noeud homologue, prenez soin de placer le noeud qui recherche cette valeur d'entité spécifique au-dessus d'elle.
{: tip}

- **Intention** : la condition la plus simple correspond à une seule et même intention. Le noeud est utilisé si l'entrée utilisateur est mappée à cette intention. Utilisez la syntaxe `#intent-name`. Par exemple, `#weather` vérifie si l'intention détectée dans l'entrée utilisateur est `weather`. Si tel est le cas, le noeud est traité. 

- **Condition spéciale** : conditions qui sont fournies avec le service et que vous pouvez utiliser pour effectuer des fonctions de dialogue de base. 

  <table>
  <tr>
    <td>Nom de condition</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>anything_else</td>
    <td>Vous pouvez utiliser cette condition à la fin du dialogue pour qu'elle soit traitée lorsque l'entrée utilisateur ne correspond à aucun autre noeud du dialogue. Le noeud **Anything else** est déclenché par cette condition. </td>
  </tr>
  <tr>
    <td>conversation_start</td>
    <td>A l'instar de **welcome**, cette condition renvoie la valeur true lors du premier tour du dialogue. Contrairement à **welcome**, elle renvoie la valeur true, que la demande initiale émise par l'application contienne ou non l'entrée utilisateur. Un noeud comportant la condition **conversation_start** peut être utilisé pour initialiser des variables contextuelles ou pour effectuer d'autres tâches au début du dialogue. </td>
  </tr>
  <tr>
    <td>false</td>
    <td>Cette condition prend toujours la valeur false. Vous pouvez l'utiliser au début d'une branche en cours de développement, afin d'empêcher son utilisation, ou comme la condition d'un noeud qui fournit une fonction de base et qui est utilisé uniquement comme cible d'une action **Jump to**. </td>
  </tr>
  <tr>
    <td>irrelevant</td>
    <td>Cette condition renvoie la valeur true si l'entrée utilisateur est déterminée comme non pertinente par le service Conversation. </td>
  </tr>
  <tr>
    <td>true</td>
    <td>Cette condition renvoie toujours la valeur true. Vous pouvez l'utiliser à la fin d'une liste de noeuds ou de réponses afin de capturer les réponses qui ne correspondaient à aucune des conditions précédentes. </td>
  </tr>
  <tr>
    <td>welcome</td>
    <td>Cette condition renvoie la valeur true lors du premier tour du dialogue (lorsque la conversation démarre), uniquement si la demande initiale émise par l'application ne contient aucune entrée utilisateur. Elle prend la valeur false dans tous les tours de dialogue suivants. Le noeud **Welcome** est déclenché par cette condition. En général, un noeud comportant cette condition est utilisé comme message d'accueil, par exemple, "Bienvenue dans notre application de commande de pizza". </td>
  </tr>
  </table>

### Syntaxe de condition

Utilisez l'une des options de syntaxe suivantes pour créer des expressions valides dans des conditions :

- Le langage d'expression SpEL, qui prend en charge l'interrogation et la manipulation d'un graphe d'objets lors de l'exécution. Pour plus d'informations, reportez-vous à l'article [Spring Expression Language (SpEL) language ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}. 

- Des notations sténographiques, qui permettent de faire référence à des intentions, des entités et des variables contextuelles. Reportez-vous à la rubrique [Accès à des objets en vue de leur évaluation](expression-language.html).

Utilisez des expressions régulières pour comparer des valeurs à des conditions. Pour trouver une chaîne correspondante, par exemple, vous pouvez utiliser la méthode `String.find`. Pour plus d'informations, reportez-vous à la rubrique [Méthodes](dialog-methods.html). 

### Conseils relatifs à l'utilisation des conditions

- Si vous souhaitez évaluer uniquement la valeur de la première instance détectée pour un type d'entité, vous pouvez utiliser la syntaxe `@entity == 'specific-value'` à la place du format `@entity:(specific-value)`. Par exemple, lorsque vous utilisez `@appliance == 'air conditioner'`, vous évaluez uniquement la valeur de la première entité `@appliance` détectée. Mais, lorsque la syntaxe `@appliance:(air conditioner)` est utilisée, elle prend la forme développée `entity['appliance'].contains('air conditioner')`, et une correspondance est établie chaque fois qu'au moins une entité `@appliance` avec la valeur 'air conditioner' est détectée dans l'entrée utilisateur. 
- Lorsque vous utilisez des variables numériques, assurez-vous qu'elles comportent des valeurs. Si une variable ne comporte pas de valeur, elle est considérée comme ayant une valeur null (0) dans une comparaison numérique. Par exemple, si vous recherchez la valeur d'une variable avec la condition `@price < 100` et que l'entité @price a une valeur null, la condition prend la valeur `true` car la valeur 0 est inférieure à 100, même si le prix n'a jamais été défini. Pour empêcher la recherche des variables null, utilisez une condition telle que `@price AND @price < 100`. Si aucune valeur n'est associée à @price, cette condition renvoie correctement la valeur false. 
- Si vous utilisez une entité comme condition et que la fonction Fuzzy Matching est activée, `@entity_name` renvoie la valeur true uniquement si la cote de confiance relative à la correspondance est supérieure à 30 %. Autrement dit, seulement si `@entity_name.confidence > .3`.

## Réponses
{: #responses}

La réponse de dialogue décrit comment répondre à l'utilisateur. 

Vous pouvez répondre en utilisant l'un des types de réponse suivants :

- [Réponse textuelle simple](#simple-text)
- [Plusieurs réponses conditionnées](#multiple)
- [Réponse complexe](#complex)

### Réponse textuelle simple
{: #simple-text}

Si vous souhaitez fournir une réponse textuelle, il vous suffit d'entrer le texte que le service doit afficher pour l'utilisateur. 

![Graphique représentant un noeud avec la question posée par un utilisateur : "Où êtes-vous situés ?" et la réponse formulée par le dialogue :  "Nous n'avons pas de point de vente physique, mais avec une connexion Internet, vous pouvez acheter nos produits depuis n'importe où"](images/response-simple.png)

#### Ajout de variations
{: #variety}

Si vos utilisateurs utilisent fréquemment votre service de conversation, ils peuvent finir par se lasser d'entendre toujours le même message d'accueil et les mêmes réponses. Vous pouvez ajouter des *variations* à vos réponses de sorte que votre conversation puisse répondre de différentes manières à une même condition. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Dans l'exemple ci-dessous, la réponse fournie par le service aux questions relatives à l'emplacement d'un magasin diffère d'une interaction à l'autre :

![Graphique représentant un noeud avec la question posée par un utilisateur : "Où êtes-vous ?" et les trois réponses différentes formulées par le dialogue"](images/variety.png)

Vous pouvez choisir de faire alterner les variations de réponse de manière séquentielle ou aléatoire. Par défaut, les réponses alternent de manière séquentielle, comme si elles avaient été choisies dans une liste ordonnée. 

### Réponses conditionnées
{: #multiple}

Un seul et même noeud de dialogue peut fournir différentes réponses, chacune d'elles étant déclenchées par une condition différente. Utilisez cette approche pour prendre en charge plusieurs scénarios dans un seul et même noeud. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Le noeud comporte toujours une condition principale, qui sert à l'utilisation du noeud et au traitement des conditions et des réponses qu'il contient. 

Dans l'exemple ci-dessous, le service utilise des informations qu'il a collectées précédemment au sujet de l'endroit où se trouve un utilisateur afin de personnaliser sa réponse, et il fournit des informations sur le magasin le plus proche de l'utilisateur. Pour savoir comment stocker les informations collectées auprès de l'utilisateur, reportez-vous à la section[Variables contextuelles](#context). 

![Graphique représentant un noeud avec la question posée par un utilisateur : "Où êtes-vous ?" et les trois réponses différentes formulées par le dialogue en fonction des conditions qui utilisent les informations de la variable contextuelle $state pour spécifier des lieux qui se trouvent dans ces départements"](images/multiple-responses.png)

A présent, ce seul et même noeud fournit les mêmes fonctions que quatre noeuds distincts. 

Les conditions définies dans un noeud sont évaluées dans un ordre défini, tout comme les noeuds. Prenez soin de spécifier vos conditions et vos réponses dans l'ordre souhaité. Si vous devez modifier cet ordre, sélectionnez une condition et déplacez-la vers le haut ou vers le bas dans la liste à l'aide des flèches qui apparaissent. Si vous souhaitez mettre à jour le contexte, vous devez le faire de manière individuelle dans chacune des réponses. Il n'existe pas de section de réponse commune. L'action **Jump to** est traitée après qu'une réponse a été sélectionnée et fournie. Si vous ajoutez une action **Jump to**, elle est exécutée après le renvoi d'une réponse à partir du noeud.
{: tip}

### Réponse complexe
{: #complex}

Pour spécifier une réponse plus complexe, vous pouvez utiliser l'éditeur JSON pour spécifier la réponse dans la propriété `"output":{}`. 

Pour inclure une valeur de variable contextuelle dans la réponse, utilisez la syntaxe `$variable-name` pour la spécifier. Pour plus d'informations, reportez-vous à la section [Variables contextuelles](#context). 

```json
{
  "output": {
    "text": "Hello $user"
  }
}
```
{: codeblock}

Pour spécifier plusieurs instructions à afficher sur des lignes distinctes, définissez la sortie sous la forme d'un tableau JSON. 

```json
{
  "output": {
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

La première phrase est affichée sur une ligne et la deuxième phrase est présentée sous la forme d'une nouvelle ligne au-dessous de la première. 

Pour implémenter un comportement plus complexe, vous pouvez définir le texte de sortie comme un objet JSON complexe. Par exemple, vous pouvez utiliser un objet complexe dans la sortie JSON afin de reproduire le comportement qui consiste à ajouter des variations de réponse au noeud. Vous pouvez inclure les propriétés suivantes dans l'objet complexe :

- **values** : tableau JSON composé de plusieurs chaînes représentant différentes versions d'un texte de sortie que ce noeud de dialogue peut renvoyer. L'ordre dans lequel les valeurs du tableau sont renvoyées dépend de l'attribut `selection_policy`.

- **selection_policy** : les valeurs suivantes sont valides :

    - **random** : le système sélectionne un texte de sortie de manière aléatoire dans le tableau `values` et ne répète pas deux fois de suite le même texte de sortie. Prenons l'exemple d'un objet output.text qui contient trois valeurs. Pour les trois premières fois, une valeur aléatoire est sélectionnée mais jamais répétée. Une fois que toutes les valeurs de sortie sont définies, le système sélectionne une autre valeur de manière aléatoire et répète le processus.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {: codeblock}

    Le système renvoie l'une de ces trois versions du message d'accueil, choisie de façon aléatoire. Au déclenchement suivant de la réponse, un autre message d'accueil pris dans cette liste s'affiche. Là encore, le message d'accueil est choisi de façon aléatoire, mais celui qui a été utilisé la fois précédente n'est volontairement pas répété. 

    - **sequential** : le système renvoie le premier texte de sortie la première fois que le noeud de dialogue est déclenché, le deuxième texte de sortie la deuxième fois que le noeud est déclenché, et ainsi de suite. 

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {: codeblock}

- **append** : indique si une valeur doit être ajoutée à un tableau ou si les valeurs du tableau doivent être écrasées par la ou les nouvelles valeurs. Lorsque cette propriété a pour valeur false, la sortie collectée dans les noeuds de dialogue précédemment exécutés est écrasée par la valeur de texte spécifiée dans ce noeud spécifique. 

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {: codeblock}

    En l'occurrence, tous les autres textes de sortie sont écrasés par ce texte de sortie. 

Les principes qui régissent le comportement par défaut sont les suivants : `selection_policy = random` et `append = true`. Lorsque le tableau de valeurs contient plus d'un élément, le texte de sortie est sélectionné de façon aléatoire dans cette liste d'éléments. 

### Définition de l'étape suivante
{: #jump-to}

Après avoir formulé la réponse spécifiée, vous pouvez demander au service d'effectuer l'une des actions suivantes :

- **Wait for user input** : le service attend que l'utilisateur fournisse une nouvelle entrée sollicitée par la réponse. Par exemple, la réponse peut poser une question à laquelle l'utilisateur doit répondre par oui ou par non. Le dialogue ne progresse pas tant que l'utilisateur ne fournit pas une autre entrée. 
- **Jump to another dialog node** : utilisez cette option pour ignorer le message réclamant une entrée utilisateur et lorsque vous souhaitez que la conversion accède directement aux noeuds enfant ou à un noeud de dialogue complètement différent. Vous pouvez utiliser une action Jump to pour acheminer le flux vers un noeud de dialogue commun à partir de plusieurs emplacements dans l'arborescence, par exemple. 
  >Remarque : avant de pouvoir configurer l'action Jump to pour un noeud cible auquel vous voulez passer directement, ce dernier doit déjà exister. 

#### Configuration de l'action Jump to

Si vous choisissez de passer directement à un autre noeud, vous devez spécifier si l'action cible la partie **réponse** ou la partie **condition** du noeud de dialogue sélectionné. 

- **Réponse** : si l'instruction cible la partie réponse du noeud de dialogue sélectionné, celle-ci est exécutée immédiatement. Autrement dit, le système n'évalue pas la partie condition du noeud de dialogue sélectionné et exécute immédiatement la partie réponse du noeud de dialogue sélectionné. 

  Cibler la réponse est utile pour enchaîner plusieurs noeuds de dialogue. La partie réponse du noeud de dialogue sélectionné est traitée comme si la condition de ce noeud de dialogue avait pour valeur true. Si le noeud de dialogue sélectionné comporte une autre action **Jump to**, celle-ci est également exécutée immédiatement. 
- **Condition** : si l'instruction cible la partie condition du noeud de dialogue sélectionné, le service vérifie d'abord si la condition du noeud ciblé renvoie la valeur true. 
    - Si la condition renvoie la valeur true, le système traite ce noeud immédiatement en mettant à jour le contexte avec le contexte du noeud de dialogue et la sortie avec la sortie du noeud de dialogue. 
    - Si la condition ne renvoie pas la valeur true, le système poursuit le processus d'évaluation sur une condition du noeud de même niveau situé juste après le noeud de dialogue cible, et ainsi de suite, jusqu'à ce qu'il trouve un noeud de dialogue avec une condition qui renvoie la valeur true. 
    - Si le système traite tous les éléments apparentés et qu'aucune condition ne renvoie la valeur true, la stratégie de rétromigration de base est utilisée et le dialogue évalue également les noeuds de niveau supérieur. 

    Cibler la condition est utile pour enchaîner les conditions de noeuds de dialogue. Par exemple, vous souhaiterez peut-être vérifier d'abord si l'entrée contient une intention, par exemple, `#turn_on`, et si tel est le cas, vous souhaiterez peut-être vérifier si l'entrée contient des entités, par exemple, `@lights`, `@radio` ou `@wipers`. Enchaîner les conditions permet de structurer des arborescences de dialogue plus vastes. 

**Remarque** : le traitement des actions **Jump to** a été modifié dans le cadre de l'édition du 3 février 2017. Pour plus d'informations, reportez-vous à la rubrique [Mise à niveau des espaces de travail ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](upgrading.html){: new_window}. 

#### Informations complémentaires

Pour obtenir des informations sur le langage d'expression utilisé par un dialogue, et sur les méthodes, les entités de système, ainsi que d'autres informations utiles, reportez-vous à la section Reference dans le panneau de navigation. 

## Variables contextuelles
{: #context}

Le dialogue est sans état, ce qui signifie qu'il ne conserve aucune information entre chaque échange avec l'utilisateur. Il revient à votre application de préserver le flux continu d'informations dont elle a besoin. Cependant, l'application peut transmettre des informations au dialogue, et le dialogue peut mettre à jour ces informations et les transmettre en retour à l'application. Pour ce faire, des variables contextuelles sont utilisées. 

Une variable contextuelle est une variable que vous définissez dans un noeud et à laquelle vous attribuez éventuellement une valeur. D'autres noeuds ou logiques d'application peuvent ensuite définir ou modifier la valeur de la variable contextuelle.  

Vous pouvez évaluer des conditions par rapport à des valeurs de variable contextuelle en référençant une variable contextuelle à partir d'une condition de noeud de dialogue afin de déterminer si un noeud doit être exécuté. Et vous pouvez référencer une variable contextuelle à partir de conditions de réponse de noeud de dialogue afin d'afficher différentes réponses en fonction d'une valeur fournie par un service externe ou par l'utilisateur. 

### Transmission de contexte à partir de l'application

Transmettez des informations de l'application au dialogue en définissant une variable contextuelle et en transmettant celle-ci au dialogue. 

Par exemple, votre application peut définir une variable contextuelle $time_of_day et la transmettre au dialogue, lequel peut utiliser cette information afin de personnaliser le message d'accueil à afficher pour l'utilisateur. 

![Graphique représentant un noeud Welcome qui utilise des conditions de réponse pour rechercher la valeur de la variable contextuelle $time_of_day qui est transmise au dialogue à partir de l'application.](images/set-context.png)

Dans cet exemple, le dialogue sait que l'application définit la variable avec l'une des valeurs suivantes : *morning*, *afternoon* ou *evening*. Il peut rechercher chaque valeur, et selon la valeur qui est présente, renvoyer le message d'accueil approprié. Si la variable n'est pas transmise ou comporte une valeur qui ne correspond à aucune des valeurs prévues, l'utilisateur voit s'afficher un message d'accueil plus générique. 

### Transmission de contexte d'un noeud à un autre

Le dialogue peut également ajouter des variables contextuelles pour transmettre des informations d'un noeud à un autre ou pour mettre à jour les valeurs de variables contextuelles. A mesure que le dialogue demande et obtient des informations auprès de l'utilisateur, il peut suivre les informations et les référencer ultérieurement dans la conversation.

Par exemple, dans un noeud, vous pouvez demander leur nom à des utilisateurs, et dans un noeud ultérieur, vous pouvez vous adresser à eux directement par leur nom. 

![Graphique représentant un noeud nommé introductions qui demande son nom à un utilisateur et stocke cette information sous forme de variable contextuelle. Le noeud suivant fait référence à l'utilisateur en le nommant au moyen de la variable contextuelle $username.](images/set-context-username.png)

Dans cet exemple, l'entité de système @sys-person est utilisée pour extraire le nom de l'utilisateur de l'entrée si l'utilisateur le communique. Dans l'éditeur JSON, la variable contextuelle $username est définie avec la valeur @sys-person. Dans un noeud suivant, la variable contextuelle $username est incluse dans la réponse pour s'adresser à l'utilisateur par son nom. 

### Définition d'une variable contextuelle

Définissez une variable contextuelle en ajoutant une paire composée d'un `nom` et d'une `valeur` à la section `{context}` de la définition de noeud de dialogue JSON. La paire doit répondre aux exigences suivantes :

- Le `nom` peut contenir des caractères alphabétiques majuscules et minuscules, des caractères numériques (0-9) et des traits de soulignement. 

  **Remarque** : vous pouvez inclure d'autres caractères, par exemple, des points et des traits d'union, dans le nom. Cependant, dans ce cas, vous devez utiliser l'une des approches suivantes pour faire référence à la variable :
  - context['variable-name']:    syntaxe d'expression SpEL complète
  - $(variable-name) : syntaxe abrégée avec le nom de variable entre parenthèses

  Pour plus d'informations, reportez-vous à la rubrique [Accès à des objets en vue de leur évaluation](expression-language.html#shorthand-syntax-for-context-variables). 

- La `valeur` peut correspondre à n'importe quel type JSON pris en charge, par exemple, une variable de chaîne simple, un nombre, un tableau JSON ou un objet JSON. 

L'exemple JSON suivant définit des valeurs pour les variables contextuelles de chaîne $dessert, de tableau $toppings_array et de nombre $age :

```json
{
  "context": {
    "dessert": "ice-cream",
    "toppings_array": ["onion", "olives"],
    "age": 18
  }
}
```
{: codeblock}

Pour définir une variable contextuelle, procédez comme suit :

1.  A partir de la vue d'édition du noeud, ouvrez l'éditeur JSON en cliquant sur l'icône ![Réponse avancée](images/kabob.png), puis en sélectionnant **JSON**.

1.  Avant le bloc `"output":{}`, ajoutez un bloc `"context":{}`, le cas échéant. 

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  Dans le bloc context, ajoutez une paire composée d'un nom et d'une valeur pour chaque variable contextuelle que vous souhaitez définir. 

    ```json
    {
      "context":{
        "name": "value"
    },
    ...
    }
    ```
    {: codeblock}

  Pour faire référence par la suite à la variable contextuelle, utilisez la syntaxe `$name` où *name* est le nom de la variable contextuelle que vous avez définie. 

Autres tâches courantes :

- Pour stocker l'intégralité de la chaîne qui a été entrée par l'utilisateur, utilisez `input.text` :

    ```json
    {
      "context": {
        "repeat": "<?input.text?>"
      }
    }
    ```
    {: codeblock}

- Pour stocker la valeur d'une entité dans une variable contextuelle, utilisez la syntaxe suivante :

    ```json
    {
      "context": {
        "place": "@place"
      }
    }
    ```
    {: codeblock}

- Pour stocker dans une variable contextuelle la valeur d'une chaîne que vous extrayez à partir de l'entrée utilisateur à l'aide d'une expression régulière, utilisez la syntaxe suivante :

    ```json
    {
      "context": {
         "number": "<?input.text.extract('^[^\\d]*[\\d]{11}[^\\d]*$',0)?>"
      }
    }
    ```
    {: codeblock}

- Pour stocker la valeur d'une entité de canevas dans une variable contextuelle, ajoutez .literal au nom d'entité. L'utilisation de cette syntaxe permet de s'assurer que le passage de texte issu d'une entrée utilisateur qui correspondait exactement au canevas spécifié est stocké dans la variable. 

    ```json
    {
      "context": {
        "email": "@email.literal"
      }
    }
    ```
    {: codeblock}

### Ordre des opérations
{: #order-of-context-var-ops}

L'ordre dans lequel vous définissez les variables contextuelles ne détermine pas l'ordre dans lequel elles sont évaluées par le service. Le service évalue de façon aléatoire les variables qui sont définies en tant que paires composées d'un nom et d'une valeur JSON. Si vous définissez une valeur dans la première variable contextuelle, ne vous attendez pas à pouvoir l'utiliser dans la deuxième, en effet, il n'y a aucune garantie que la première variable contextuelle de votre liste sera exécutée avant la deuxième. Par exemple, n'utilisez pas deux variables contextuelles pour implémenter une logique qui renvoie un nombre aléatoire compris entre zéro et une valeur supérieure qui est transmise au noeud. 

```json
"context": {
    "upper": "<? @sys-number.numeric_value + 1?>",
    "answer": "<? new Random().nextInt($upper) ?>"
}
```
{: codeblock}

Utilisez une expression légèrement plus complexe pour éviter d'avoir à vous baser sur la valeur de la variable contextuelle $upper en cours d'évaluation avant que la variable contextuelle $answer ne soit évaluée. 

```json
"context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  }
```
{: codeblock}

### Mise à jour d'une valeur de variable contextuelle
{: #updating-a-context-variable-value}

Si un noeud définit la valeur d'une variable contextuelle qui est déjà définie, la valeur précédente est écrasée. 

#### Mise à jour d'un objet JSON complexe

Les valeurs précédentes sont écrasées pour tous les types JSON, sauf un objet JSON. Si la variable contextuelle est un type complexe, par exemple, un objet JSON, une procédure de fusion JSON est utilisée pour mettre à jour la variable. L'opération de fusion ajoute les propriétés nouvellement définies pour l'objet et écrase les propriétés existantes. 

Dans l'exemple ci-dessous, une variable contextuelle de nom est définie en tant qu'objet complexe :

```json
{
  "context": {
    ...
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan"
      "has_card" : false
    }
  }
}
```
{: codeblock}

Un noeud de dialogue met à jour l'objet JSON de variable contextuelle avec les valeurs suivantes :

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

Le résultat obtenu est le contexte suivant :

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

#### Mise à jour de tableaux

Si les données contextuelles de votre dialogue contiennent un tableau de valeurs, vous pouvez mettre à jour ce tableau en ajoutant des valeurs, en retirant des valeurs ou en remplaçant toutes les valeurs. 

Choisissez l'une des actions répertoriées ci-après pour mettre à jour le tableau. Pour chaque cas, le tableau est décrit avant et après l'action, et entre ces deux descriptions, l'action proprement dite est présentée. 

- **Action d'ajout** : pour ajouter des valeurs à la fin d'un tableau, utilisez la méthode `append`. 

    Pour le contexte d'exécution de dialogue suivant :

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
    ```
    {: codeblock}

    Effectuez la mise à jour suivante :

    ```json
    {
      "context": {
        "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
      }
    }
    ```
    {: codeblock}

    Résultat :

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
      }
    }
    ```
    {: codeblock}

- **Action de retrait** : pour retirer un élément, utilisez la méthode `remove` et spécifiez sa valeur ou sa position dans le tableau. 

    - **Action de retrait par valeur** : permet de retirer un élément d'un tableau en utilisant sa valeur. 

        Pour le contexte d'exécution de dialogue suivant :

        ```json
        {
          "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
        ```
        {: codeblock}

        Effectuez la mise à jour suivante :

        ```json
        {
          "context": {
        "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
          }
        }
        ```
        {: codeblock}

        Résultat :

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

    - **Action de retrait par position** : permet de retirer un élément d'un tableau en utilisant sa position dans l'index.

        Pour le contexte d'exécution de dialogue suivant :

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
      }
    }
        ```
        {: codeblock}

        Effectuez la mise à jour suivante :

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.remove(0) ?>"
          }
        }
        ```
        {: codeblock}

        Résultat :

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

- **Action d'écrasement** : pour écraser les valeurs contenues dans un tableau, il suffit de définir le tableau avec les nouvelles valeurs.

    Pour le contexte d'exécution de dialogue suivant :

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
      }
    }
        ```
        {: codeblock}

    Effectuez la mise à jour suivante :

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    Résultat :

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

**Attention** : si vous sauvegardez le tableau en tant que partie d'une chaîne, il n'est plus un tableau, mais devient un objet de chaîne. Par exemple, la variable contextuelle $array suivante est un tableau, mais la variable contextuelle $string_array est une chaîne. 

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

Si vous utilisez le panneau Try it out pour vérifier les valeurs de ces variables contextuelles, ces valeurs sont spécifiées comme suit :

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

Vous pouvez ensuite exécuter des méthodes de tableau sur la variable $array, par exemple, `<? $array.removeValue('two') ?>`, mais pas sur la variable $array_in_string. 

## Création d'un dialogue
{: #create}

Utilisez l'outil {{site.data.keyword.conversationshort}} pour créer un dialogue. 

### Nombre limite de noeuds de dialogue
{: #dialog-node-limits}

Le nombre de noeuds de dialogue que vous pouvez créez dépend de votre plan de service. 

| Plan de service  | Nombre de noeuds de dialogue par espace de travail |
|------------------|---------------------------:|
| Standard/Premium |                    100 000 |
| Lite             |                     25 000 |

Profondeur d'arborescence maximale : le service prend en charge 2 000 descendants de noeud de dialogue ; les outils sont plus performants si ce nombre n'excède pas 20. 

### Procédure

Pour créer un dialogue, procédez comme suit :

1.  Ouvrez la page **Build** dans la barre de navigation, cliquez sur l'onglet **Dialog**, puis sur **Create**.

    Lorsque vous ouvrez le générateur de dialogue pour la première fois, les noeuds suivants sont déjà créés pour vous :
    - **Welcome** : permier noeud. Il contient un message d'accueil qui s'affiche lorsque vos utilisateurs interagissent pour la première fois avec le service. Le message d'accueil peut être édité. 
    - **Anything else** : noeud final. Il contient les phrases qui sont utilisées pour répondre aux utilisateurs lorsque leur entrée n'est pas reconnue. Il est possible de remplacer les réponses fournies ou d'en ajouter d'autres ayant une signification similaire afin d'ajouter de la diversité à la conversation. Vous pouvez également spécifier si le service doit renvoyer chacune des réponses définies les unes après les autres ou si elles doivent être renvoyées de façon aléatoire. 
1.  Pour ajouter d'autres noeuds à l'arborescence de dialogue, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le noeud **Welcome**, puis sélectionnez **Add node below**.
1.  Entrez une condition, qui, lorsqu'elle est satisfaite, déclenche le traitement du noeud par le service. 

    Lorsque vous commencez à définir une condition, une zone comportant les options possibles s'affiche. Vous pouvez entrer l'un des caractères suivants, puis choisir une valeur dans la liste des options qui s'affiche.

    <table>
    <tr>
      <td>Caractère</td>
      <td>Valeurs définies dans des listes pour ces types d'artefact</td>
    </tr>
    <tr>
      <td>`#`</td>
      <td>intentions</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>entités</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>valeurs {entity-name}</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>variables contextuelles que vous avez définies ou référencées ailleurs dans le dialogue</td>
    </tr>
    </table>

    Vous pouvez créer une nouvelle intention, entité, valeur d'entité ou variable contextuelle en définissant une nouvelle condition qui l'utilise. Si vous créez un artefact de cette façon, prenez soin de revenir en arrière et d'effectuer les autres étapes requises pour la création complète de l'artefact, par exemple, en définissant des exemples d'énoncé pour une intention. 

    Pour définir un noeud qui se déclenche en fonction de plusieurs conditions, entrez une condition, puis cliquez sur le signe plus (+) en regard de celle-ci. Si vous souhaitez appliquer un opérateur `OR` aux conditions multiples au lieu de l'opérateur `AND`, cliquez sur la mention `and` affichée entre les zones pour modifier le type d'opérateur. Les opérations AND sont exécutées avant les opérations OR, mais vous pouvez modifier l'ordre en utilisant des parenthèses. Par exemple :
    `$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    La condition que vous définissez ne doit pas excéder 500 caractères. 

    Pour savoir comment tester les valeurs définies dans des conditions, reportez-vous à la rubrique [Conditions](#conditions).
1.  **Facultatif** : si vous souhaitez collecter plusieurs éléments d'informations auprès de l'utilisateur au sein de ce noeud, cliquez sur **Customize**, puis activez **Slots**. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](#slots). 
1.  Entrez une réponse.
    - Ajoutez le texte que le service doit afficher comme réponse. 
    - Pour plus d'informations sur les réponses conditionnelles et pour savoir comment ajouter de la diversité aux réponses ou comment spécifier ce qui doit se produire après le déclenchement du noeud, reportez-vous à la section [Réponses](#responses).
1.  **Facultatif** : donnez un nom au noeud.

    Le nom de noeud de dialogue peut contenir des lettres (au format Unicode), des nombres, des espaces, des traits de soulignement, des traits d'union et des points. 

    Le fait de nommer un noeud permet de mémoriser plus facilement son objectif et de le localiser plus facilement lorsqu'il est réduit. Si vous n'indiquez pas de nom, la condition du noeud est utilisée comme nom. 

1.  Pour ajouter d'autres noeuds, sélectionnez un noeud dans l'arborescence, puis cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png). 
    - Pour créer un noeud homologue qui est vérifié ensuite si la condition relative au noeud existant n'est pas satisfaite, sélectionnez **Add node below**.
    - Pour créer un noeud homologue qui est vérifié avant la condition relative au noeud existant, sélectionnez **Add node above**.
    - Pour créer un noeud enfant sur le noeud sélectionné, sélectionnez **Add child node**. Un noeud enfant est traité après son noeud parent.

    Pour plus d'informations sur l'ordre dans lequel les noeuds de dialogue sont traités, reportez-vous à la section [Présentation du dialogue](#overview).
1.  Testez le dialogue à mesure que vous le créez.
   Pour plus d'informations, reportez-vous à la section [Test de votre dialogue](#test). 

## Collecte d'informations à l'aide d'attributs
{: #slots}

Ajoutez des attributs à un noeud de dialogue afin de collecter plusieurs éléments d'informations auprès d'un utilisateur au sein de ce noeud. Les attributs permettent de collecter des informations au rythme de l'utilisateur. Les détails fournis initialement par l'utilisateur sont sauvegardés et le service réclame uniquement les détails qui n'ont pas été fournis. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ES4GHcDsSCI?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### Pourquoi ajouter des attributs ?
{: #why-add-slots}

Les attributs vous permettent d'obtenir les informations dont vous avez besoin pour répondre de manière précise à l'utilisateur. Par exemple, si un utilisateur demande des horaires d'ouverture et de fermeture et que ces informations varient en fonction des différents sites de magasin, vous pouvez poser une question complémentaire afin de demander à l'utilisateur sur quel site il souhaite se rendre, avant de lui répondre. Vous pouvez ensuite ajouter des conditions de réponse qui tiennent compte des informations de site fournies. 

![Illustration représentant une question posée à l'utilisateur pour lui demander de préciser le site de magasin avant de répondre à la question : When do you open?.](images/op-hours.png)

Les attributs peuvent vous aider à collecter plusieurs éléments d'informations dont vous avez besoin afin d'effectuer une tâche complexe pour un utilisateur, par exemple, réserver une table dans un restaurant. 

![Illustration représentant quatre attributs qui invitent à saisir les informations nécessaires pour réserver une table dans un restauration](images/reservation.png)

L'utilisateur peut fournir des valeurs pour plusieurs attributs en même temps. Par exemple, l'entrée peut inclure les informations suivantes : "C'est une réservation pour 6 personnes à 19 heures." Cette entrée contient deux des valeurs requises manquantes : le nombre de personnes et l'heure de la réservation. Le service reconnaît et stocke ces deux valeurs, chacune dans son attribut correspondant. Il affiche ensuite l'invite qui est associée au prochain attribut vide. 

![Illustration montrant que deux attributs sont renseignés et que le service demande des informations pour un autre attribut.](images/pass-in-info.png)

Les attributs permettent au service de répondre à des questions complémentaires sans avoir à redéfinir l'objectif de l'utilisateur. Par exemple, un utilisateur peut demander des prévisions météorologiques, puis poser une question complémentaire portant sur la météo pour un autre endroit ou un autre jour. Imaginons que vous sauvegardiez les variables de prévision requises, comme l'endroit et le jour, dans des attributs, si un utilisateur poste une question complémentaire afin d'obtenir de nouvelles valeurs de variable, vous pouvez écraser les valeurs d'attribut avec les nouvelles valeurs fournies et donner une réponse qui reflète les nouvelles informations. 

![Illustration représentant une question portant sur des prévisions météorologiques, suivie d'une question complémentaires portant sur la météo pour un autre lieu et un autre moment. ](images/follow-up.png)

L'utilisation d'attributs permet de produire un flux de dialogue plus naturel entre l'utilisateur et le service et est plus facile à gérer que l'opération consistant à essayer de collecter les informations en utilisant un grand nombre de noeuds distincts. 

#### Ajout d'attributs
{: #add-slots}

1.  Identifiez les unités d'informations que vous souhaitez collecter. Par exemple, pour commander une pizza pour quelqu'un, vous souhaiterez peut-être collecter les informations suivantes : 

    - Heure de livraison
    - Taille

1.  Dans la vue d'édition du noeud de dialogue, cliquez sur **Customize**, puis cochez la case **Slots**. 

    **Remarque** : pour plus d'informations sur la zone **Prompt for everything**, reportez-vous à la section  [Procédure permettant de demander toutes les informations en même temps](dialog-build.html#slots-prompt-for-everything).

1.  **Ajoutez un attribut pour chaque unité d'informations requises**.

    Pour chaque attribut, spécifiez les détails suivants :

    - **Check for** : identifiez le type d'information que vous souhaitez extraire de la réponse de l'utilisateur afin de la transmettre pour la demande d'attribut. La plupart du temps, vous recherchez des valeurs d'entité, mais vous pouvez aussi rechercher une intention. Vous pouvez utiliser des opérateurs AND et OR ici pour définir des conditions plus complexes. 

      **Remarque** : si des canevas sont définis pour l'entité, après avoir ajouté le nom d'entité, ajoutez-lui `.literal`. Par exemple, après avoir choisi `@email` dans la liste d'entités définies, éditez la zone *Check for* pour qu'elle contienne `@email.literal`. En ajoutant la propriété `.literal`, vous indiquez que vous souhaitez capturer le texte exact entré par l'utilisateur et qui a été identifié comme adresse électronique sur la base de son canevas. 

      Evitez de rechercher des valeurs de variable contextuelle. La valeur *Check for* est d'abord utilisée comme condition, puis elle devient la valeur de la variable contextuelle que vous indiquez dans la zone *Save as*. Si vous utilisez une variable contextuelle dans la condition, cela peut entraîner un comportement inattendu lorsqu'elle est utilisée dans le contexte.
      {: tip}

    - **Save as** : indiquez le nom de la variable contextuelle dans laquelle vous souhaitez stocker la valeur d'intérêt provenant de la réponse de l'utilisateur et à transmettre à la demande d'attribut. Ne spécifiez pas une variable contextuelle qui a été utilisée précédemment dans le dialogue, et qui par conséquent, est susceptible de posséder une valeur. La demande d'attribut s'affiche uniquement lorsque la variable contextuelle pour l'attribut a pour valeur null. 

    - **Prompt** : écrivez une instruction qui sollicite l'élément d'information dont vous avez besoin auprès de l'utilisateur. Après l'affichage de cette invite, la conversation marque une pause et le service attend la réponse de l'utilisateur.

    - Si vous éditez l'attribut, vous pouvez également définir les réponses qui doivent s'afficher après que l'utilisateur a répondu à la demande d'attribut. 
      - **Found** : exécuté après que l'utilisateur a fourni les informations attendues. 
      - **Not found** : exécuté si les informations fournies par l'utilisateur ne sont pas comprises ou si elles ne sont pas fournies dans le format attendu. Le texte que vous indiquez ici peut stipuler de manière plus explicite le type d'information dont vous avez besoin de la part de l'utilisateur. Si l'attribut est correctement renseigné, ou si l'entrée utilisateur est comprise et gérée par un gestionnaire de niveau noeud, cette condition ne se déclenche jamais. 

    Le tableau ci-dessous contient des exemples de valeur d'attribut pour un noeud permettant aux utilisateurs de commander une pizza :

    <table>
    <tr>
      <td>Information</td>
      <td>Zone Check for</td>
      <td>Zone Save as</td>
      <td>Zone Prompt</td>
      <td>Réponse de suivi si la valeur est trouvée (condition If Found)</td>
      <td>Réponse de suivi si la valeur n'est pas trouvée (condition If Not Found)</td>
    </tr>
    <tr>
      <td>Taille</td>
      <td>@size</td>
      <td>$size</td>
      <td>"Quelle taille de pizza souhaitez-vous ?</td>
      <td>"$size, très bien."</td>
      <td>"Quelle taille souhaitiez-vous ? Nous proposons des pizza petites, moyennes et grandes. </td>
    </tr>
    <tr>
      <td>Délai de livraison</td>
      <td>@sys-time</td>
      <td>$time</td>
      <td>"Quand souhaitez-vous être livré ?"</td>
      <td>"Pour une livraison à $time."</td>
      <td>"A quelle heure souhaitiez-vous être livré ? Il faut compter au moins 30 minutes de préparation."</td>
    </tr>
    </table>

    **Attributs facultatifs** : si vous souhaitez ajouter un attribut qui capture des informations mais qui est facultatif, ne spécifiez pas d'invite pour cet attribut. 

    Par exemple, vous pouvez ajouter un attribut qui capture des informations relatives à des restrictions alimentaires si l'utilisateur spécifie des restrictions. Cependant, vous ne pouvez pas demander à tous les utilisateurs d'indiquer des restrictions alimentaires car cela n'est pas pertinent dans la plupart des cas. 

    <table>
    <tr>
      <td>Information</td>
      <td>Zone Check for</td>
      <td>Zone Save as</td>
    </tr>
    <tr>
      <td>Restriction relative au gluten</td>
      <td>@dietary</td>
      <td>$dietary</td>
    </tr>
    </table>

    Lorsque vous ajoutez un attribut sans spécifier d'invite, le service interprète l'attribut comme facultatif.

    Si vous rendez un attribut facultatif, vous devez uniquement référencer sa variable contextuelle dans le texte de la réponse de niveau noeud si vous pouvez la libeller de telle manière qu'elle soit compréhensible même si aucune valeur n'est fournie pour l'attribut. Par exemple, vous pouvez libeller une instruction récapitulative, telle que "Je voudrais commander une $size pizza $dietary et être livré à $time." Le texte obtenu reste compréhensible même si les restrictions alimentaires, par exemple, `sans gluten` ou `sans lactose`, ne sont pas fournies : "Je voudrais commander une grande pizza et être livré à 15h."
    {: tip}
1.  **Assurez le suivi des utilisateurs**.
    Vous pouvez éventuellement définir des gestionnaires de niveau noeud qui fournissent des réponses aux questions que peuvent se poser les utilisateurs lors de l'interaction et qui sont tangentielles par rapport à l'objectif du noeud. 

    Par exemple, l'utilisateur peut demander la recette de la sauce tomate ou l'endroit où vous vous êtes procuré les ingrédients. Pour gérer de telles digressions, cliquez sur le lien **Manage handlers** et ajoutez une condition et une réponse pour chaque question anticipée. 

    ![Graphique représentant la question posée par un utilisateur : Quelle est la recette de votre sauce ? et la réponse donnée : Il n'est pas question que je la dévoile.](images/sauce.png)

    Après la réponse à la digression, l'invite associée à l'attribut non renseigné s'affiche. 

    Cette condition est déclenchée si l'utilisateur fournit une entrée qui correspond tout le temps aux conditions du gestionnaire pendant le flux du noeud de dialogue jusqu'à ce que la réponse de niveau noeud s'affiche.
1.  **Ajoutez une réponse de niveau noeud**.
    Cette réponse de niveau noeud n'est pas exécutée tant que tous les attributs requis ne sont pas renseignés. Vous pouvez ajouter une réponse qui récapitule les informations que vous avez collectées. Par exemple, "Une `$size` pizza vous sera livrée à `$time`. Bon appétit !"

1.  **Ajoutez une logique qui réinitialise les variables contextuelles d'attribut**.
    A mesure que vous collectez les réponses de l'utilisateur pour chaque attribut, elles sont sauvegardées dans des variables contextuelles. Vous pouvez utiliser les variables contextuelles pour transmettre les informations à un autre noeud ou à une application ou à un service externe en vue de leur utilisation. Cependant, après avoir transmis les informations, vous devez affecter la valeur null aux variables contextuelles afin de réinitialiser le noeud pour qu'il recommence à collecter des informations. Vous ne pouvez pas affecter la valeur null aux variables contextuelles au sein du noeud en cours car le service ne quittera pas le noeud tant que les attributs requis ne seront pas renseignés. En revanche, songez à utiliser l'une des méthodes suivantes : 
    - Ajoutez un traitement à l'application externe qui affecte la valeur null aux variables. 
    - Ajoutez un noeud enfant qui affecte la valeur null aux variables.
    - Insérez un noeud parent qui affecte la valeur null aux variables, puis passez au noeud comportant avec des attributs. 

Examinez les approches recommandées décrites ci-après pour gérer des tâches courantes. 

#### Procédure permettant de demander toutes les informations en même temps
{: #slots-prompt-for-everything}

Incluez une invite initiale pour l'ensemble du noeud afin d'indiquer clairement à l'utilisateur quelles sont les unités d'information qu'il doit fournir. L'affichage de cette invite initiale permet aux utilisateurs de fournir tous les détails en une seule fois et ne pas avoir à attendre d'être invité à fournir un élément d'information à la fois. 

Par exemple, lorsque le noeud est déclenché parce qu'un client souhaite commander une pizza, vous pouvez répondre avec l'invite préliminaire suivante : "Je peux prendre votre commande. Indiquez-moi la taille de la pizza que vous désirez et l'heure à laquelle vous souhaitez être livré."

Si l'utilisateur fournit ne serait-ce qu'une partie de ces informations dans sa demande initiale, l'invite n'apparaît pas. Par exemple, l'entrée initiale peut être "Je voudrais commander une grande pizza." Lorsque le service analyse l'entrée, il reconnaît "grande" comme étant la taille de la pizza et il renseigne l'attribut **Taille** avec la valeur fournie. Etant donné que l'un des attributs est renseigné, le service ignore l'affichage de l'invite initiale pour éviter de redemander la taille de la pizza. En revanche, il affiche les invites pour les autres attributs qui sont pas renseignés. 

Sur le panneau Customize où vous avez activé la fonction Slots, cochez la case **Prompt for everything** pour activer l'invite initiale. Ce paramètre ajoute la zone **If no slots are pre-filled, ask this first** au noeud, dans laquelle vous pouvez spécifier le texte qui invite l'utilisateur à spécifier toutes les informations. 

#### Procédure permettant de capturer plusieurs valeurs
{: #slots-multiple-entity-values}

Vous pouvez demander une liste d'éléments et les sauvegarder dans un attribut. 

Par exemple, vous pouvez demander aux utilisateurs s'ils souhaitent de la garniture sur leur pizza. Pour cela, définissez une entité (@toppings), et la valeur acceptée pour cette dernière (pepperoni, fromage, champignons, etc.). Ajoutez un attribut qui demande à l'utilisateur quelle garniture il désire. Utilisez la propriété values du type d'entité pour capturer plusieurs valeurs, le cas échéant. 

<table>
<tr>
  <td>Information</td>
  <td>Zone Check for</td>
  <td>Zone Save as</td>
  <td>Zone Prompt</td>
  <td>Réponse de suivi si la valeur est trouvée (condition If Found)</td>
  <td>Réponse de suivi si la valeur n'est pas trouvée (condition If Not Found)</td>
</tr>
<tr>
  <td>Garniture</td>
  <td>@toppings.values</td>
  <td>$toppings</td>
  <td>Désirez-vous une garniture ?</td>
  <td>"Excellent choix !"</td>
  <td>"Quelle garniture désirez-vous ? Nous vous proposons ..."</td>
</tr>
</table>

Afin de référencer ultérieurement les garnitures spécifiées par l'utilisateur, utilisez la syntaxe `<? $entity-name.join(',') ?>` pour répertorier chaque élément dans le tableau des garnitures en séparant les valeurs par une virgule. Par exemple, "Je voudrais commander une $size pizza avec `<? $toppings.join(',') ?>` et être livré à $time."

#### Procédure permettant de reformater des valeurs
{: #slots-reformat-values}

Etant donné que vous demandez à l'utilisateur de fournir des informations et que devez référencer leur entrée dans des réponses, songez à reformater les valeurs de manière à pouvoir les afficher dans un format convivial. 

Par exemple, les valeurs d'heure sont sauvegardées au format `hh:mm:ss`. Vous pouvez utiliser l'éditeur JSON pour l'attribut afin de reformater la valeur d'heure lorsque vous la sauvegardez de sorte que le format `hour:minutes AM/PM` soit utilisé :

```json
{
  "context":{
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Pour plus d'informations sur le reformatage des idées, reportez-vous à la rubrique [Méthodes de traitement de valeurs](dialog-methods.html). 

#### Procédure permettant d'obtenir une confirmation
{: #slots-get-confirmation}

Ajoutez un attribut au-dessous des autres attributs afin de demander à l'utilisateur de confirmer que les informations que vous avez collectées sont exactes et complètes. L'attribut peut rechercher les réponses qui correspondent à l'intention #yes. 

<table>
<tr>
  <td>Information</td>
  <td>Zone Check for</td>
  <td>Zone Save as</td>
  <td>Zone Prompt</td>
  <td>Réponse de suivi si la valeur est trouvée (condition If Found)</td>
  <td>Réponse de suivi si la valeur n'est pas trouvée (condition If Not Found)</td>
</tr>
<tr>
  <td>Confirmation</td>
  <td>#yes</td>
  <td>$confirmation</td>
  <td>"Vous avez commandé une `$size` pizza pour une livraison à `$time`. Vous confirmez votre commande ?"</td>
  <td>"C'est parti !"</td>
  <td>Voir ci-dessous</td>
</tr>
</table>

Etant donné que les utilisateurs peuvent inclure des instructions affirmatives à d'autres moments durant le dialogue (*Tout à fait, nous voulons être livré à 17h*), utilisez la propriété `slot_in_focus` pour préciser dans la condition d'attribut que vous recherchez une réponse Yes uniquement pour l'invite de cet attribut. 

```json
#yes && slot_in_focus
```
La propriété `slot_in_focus` prend toujours une valeur booléenne (true ou false). Vous devez l'inclure uniquement dans une condition pour laquelle vous souhaitez obtenir un résultat booléen. Vous ne devez pas l'utiliser dans des conditions d'attribut qui recherchent un type d'entité, puis qui sauvegardent la valeur d'entité, par exemple.
{: tip}

Dans l'invite **Not Found**, redemandez toutes les informations, puis réinitialisez les variables contextuelles que vous avez précédemment sauvegardées. 

```json
{
  "output":{
    "text": {
      "values": [
        "Let's try this again. Tell me what size pizza you want and the time..."
      ]
    }
  },
  "context":{
    "size": null,
    "time": null
  }
}
```
{: codeblock}

#### Procédure permettant de remplacer une valeur de variable contextuelle d'attribut
{: #slots-found-handler-event-properties}

Si, avant de quitter un noeud comportant des attributs, un utilisateur fournit une nouvelle valeur pour un attribut, cette nouvelle valeur est sauvegardée dans la variable contextuelle d'attribut et remplace la valeur qui avait été spécifiée précédemment. Votre dialogue reconnaît ce remplacement de manière explicite en utilisant des propriétés spéciales qui sont définies pour le gestionnaire d'événements de condition Found :

- `event.previous_value` : précédente valeur de la variable contextuelle pour cet attribut. 
- `event.current_value` : valeur en cours de la variable contextuelle pour cet attribut. 

Par exemple, votre dialogue demande une ville de destination pour une réservation de billet d'avion. L'utilisateur indique `Paris.` Vous affectez à la variable contextuelle d'attribut $destination la valeur *Paris*. Ensuite, l'utilisateur dit : `Attendez. Finalement, je veux un billet d'avion pour Madrid.` Si vous configurez la condition Found comme suit, votre dialogue peut facilement gérer ce type de modification : 

```json
When user responds, if @destination is found:
Condition: event.previous_value != null
    Response: Très bien, nous remplaçons la destination <? event.previous_value ?> par <? event.current_value ?>.
Response: Ok, destination is $destination.literal.
```

Cette configuration d'attribut permet à votre dialogue de réagir au changement de destination de l'utilisateur en disant : `Très bien, nous remplaçons la destination Paris par Madrid.`

#### Procédure permettant d'éviter de confondre des nombres
{: #slots-avoid-number-confusion}

Certaines valeurs fournies par les utilisateurs peuvent être identifiées comme plusieurs types d'entité.

Il est possible d'avoir deux attributs avec le même type de valeur, par exemple, une date d'arrivée et une date de départ. Créez une logique dans vos conditions d'attribut afin de distinguer ces valeurs similaires entre elles. 

De plus, le service peut reconnaître plusieurs types d'entité dans une seule entrée utilisateur. Par exemple, lorsqu'un utilisateur fournit une devise, celle-ci est reconnue à la fois comme un type d'entité @sys-currency et comme un type d'entité @sys-number. Effectuez des tests à l'aide du panneau "Try it out" pour comprendre de quelle façon le système interprétera différentes entrées utilisateur et créez une logique dans vos conditions afin d'empêcher toute mauvaise interprétation. 

Dans une logique qui est spécifique à la fonction d'attributs, lorsque deux entités sont reconnues dans une seule entrée utilisateur, celle dont la portée est la plus étendue est utilisée. Par exemple, si l'utilisateur entre *2 mai*, même si le service Conversation reconnaît à la fois les entités @sys-date (05022017) et @sys-number (2) dans le texte, seule l'entité dont la portée est la plus étendue (@sys-date) est enregistrée et appliquée à un attribut, le cas échéant.
{: tip}

#### Procédure permettant d'empêcher l'affichage d'une réponse Found lorsque celle-ci est inutile
{: #slots-stifle-found-responses}

Lorsque vous spécifiez des réponses Found pour plusieurs attributs, si un utilisateur indique en même temps des valeurs pour plusieurs attributs, la réponse Found pour au moins l'un d'entre eux apparaîtra. Vous souhaiterez probablement que la réponse Found soit renvoyée pour tous les attributs ou pour aucun d'entre eux. 

Pour empêcher que des réponses Found ne s'affichent, vous pouvez effectuer l'une des opérations suivantes sur chaque réponse Found :

- Ajoutez une condition à la réponse afin de l'empêcher d'apparaître si des attributs spécifiques sont renseignés. Par exemple, vous pouvez ajouter une condition, telle que `!($size && $time)`, qui empêche la réponse d'apparaître si les variables contextuelles $size et $time sont toutes les deux fournies. 
- Ajoutez la condition `!all_slots_filled` à la réponse. Ce paramètre empêche la réponse d'apparaître si tous les attributs sont renseignés. N'utilisez pas cette approche si vous ajoutez un attribut de confirmation. L'attribut de confirmation est également un attribut, et en général, vous souhaitez empêcher que les réponses Found ne s'affichent avant que l'attribut de confirmation proprement dit soit renseigné. 

#### Procédure permettant de traiter les demandes de sortie du processus
{: #slots-node-level-handler}

Ajoutez au moins un gestionnaire de niveau noeud qui peut reconnaître qu'un utilisateur souhaite quitter le noeud. 

Par exemple, dans un noeud qui collecte des informations afin de planifier un rendez-vous de toilettage pour un animal domestique, vous pouvez ajouter un gestionnaire de niveau noeud qui définit des conditions sur l'intention #cancel, qui reconnaît des énoncés tels que "Oubliez ça. J'ai changé d'avis."

1.  Dans l'éditeur JSON pour le gestionnaire, renseignez toutes les variables contextuelles d'attribut avec des valeurs factices afin d'empêcher le noeud de continuer à demander d'éventuelles valeurs manquantes. Et, dans la réponse du gestionnaire, ajoutez un message comme "Entendu. Nous allons nous arrêter là. Aucun rendez-vous ne sera pris."
1.  Dans le message de niveau noeud, ajoutez une condition qui recherche une valeur factice dans l'une des variables contextuelles d'attribut. Si cette valeur est trouvée, un message final tel que "Si vous décidez de prendre un autre rendez-vous plus tard, dites-le moi." s'affiche. Si cette valeur n'est pas trouvée, le message récapitulatif standard pour le noeud s'affiche, par exemple, "J'ai pris un rendez-vous pour votre $animal à $time le $date."
1.  Tenez compte de la logique utilisée dans les conditions qui sont évaluées avant ces gestionnaires de niveau noeud afin de pouvoir créer des conditions distinctes dans ces derniers. Lorsqu'une entrée utilisateur est reçue, les conditions sont évaluées dans l'ordre suivant : 

    - Conditions If Found du niveau d'attribut en cours.
    - Gestionnaires de niveau noeud dans l'ordre où ils sont répertoriés.
    - Conditions If Not Found du niveau d'attribut en cours.

Soyez prudent lorsque vous ajoutez des conditions qui renvoient toujours la valeur true (comme les conditions spéciales `true` ou`anything_else`) comme gestionnaires de niveau noeud. Pour chaque attribut, si le gestionnaire de niveau noeud renvoie la valeur true, la condition If Not Found est complètement ignorée. Par conséquent, le fait d'utiliser un gestionnaire de niveau noeud qui renvoie toujours la valeur true empêche réellement la condition If Not Found pour chaque attribut d'être évaluée.
{: tip}

Par exemple, vous toilettez tous les animaux, sauf les chats. Pour l'attribut Animal, vous pouvez être tenté d'utiliser la condition d'attribut suivante pour empêcher la valeur `cat` d'être sauvegardée dans l'attribut Animal : 

```json
Check for @animal && !@animal:cat, then save it as $animal.
```
{: codeblock}

Et, pour permettre aux utilisateurs de savoir que vous n'acceptez pas les chats, vous pouvez spécifier la valeur suivante dans la condition Not Found de l'attribut Animal : 

```json
If @animal && !@animal:cat then, "Je suis désolé. Nous ne faisons pas le toilettage pour chats."
```
{: codeblock}

Bien que logique, si vous définissez également un gestionnaire de demande de sortie de niveau noeud, étant donné l'ordre d'évaluation des conditions, il est fort probable que cette condition Not Found ne soit jamais déclenchée. A la place, vous pouvez utiliser la condition d'attribut suivante :

```json
Check for @animal, then save it as $animal.
```
{: codeblock}

Et, pour faire face à une éventuelle réponse `cat`, ajoutez la valeur suivante à la condition Found :

```josn
If @animal:cat then, "Je suis désolé. Nous ne faisons pas le toilettage pour chats."
```
{: codeblock}

Dans l'éditeur JSON pour la condition Found, réinitialisez la valeur de la variable contextuelle $animal car sa valeur actuelle est cat et cela ne devrait pas être le cas. 

```json
{
  "output":{
    "text": {
      "values": [
        "Je suis désolé. Nous ne faisons pas le toilettage pour chats."
      ]
    }
  },
  "context":{
    "animal": null
  }
}
```
{: codeblock}

Ci-dessous, un objet JSON définit un gestionnaire de niveau noeud pour l'exemple de la pizza :

```json
{
"conditions": "#cancel",
 "output": {
   "text": {
      "values": [
       "Ok, nous allons nous arrêter là. Aucune pizza ne sera livrée."
     ],
      "selection_policy": "sequential"
    }
  },
"context": {
   "time": "12:00:00",
   "size": "dummy",
   "confirmation":"true"
}
}
```

#### Exemples d'attributs

Pour accéder à des fichiers JSON qui implémentent différents scénarios d'utilisation d'attribut courants, accédez au [référentiel de la communauté pour Conversation ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud/community/tree/master/conversation){: new_window} dans GitHub.

Pour explorer un exemple, téléchargez l'un des exemples de fichier JSON, puis importez-le en tant que nouvel espace de travail. A partir de l'onglet Dialog, vous pouvez passer en revue les noeuds de dialogue afin de voir de quelle façon les attributs ont été implémentés pour prendre en charge les différents scénarios d'utilisation. 

## Test de votre dialogue
{: #test}

Si vous apportez des modifications à votre dialogue, vous pouvez le tester à tout moment pour voir de quelle façon il répond à une entrée. 

1.  Dans l'onglet Dialog, cliquez sur l'icône ![Demander à Watson](images/ask_watson.png). 
1.  Dans le panneau de discussion, tapez un texte et appuyez sur Entrée. 

    Avant de tester le dialogue, assurez-vous que le système a terminé de s'entraîner avec les dernières modifications que vous avez effectuées. Si tel n'est pas le cas, un message s'affiche dans la partie supérieure du panneau de discussion :
    {: tip}

    ![Capture d'écran du message indiquant que Watson est en train de s'entraîner](images/training.png)
1.  Vérifiez la réponse pour voir si le dialogue a correctement interprété votre entrée et a choisi la bonne réponse. 

    La fenêtre de discussion indique quels objectifs et quelles entités ont été reconnus dans l'entrée :

    ![Capture d'écran illustrant la sortie du dialogue de test](images/test_dialog_output.png)

    Dans le panneau de l'éditeur de dialogue, le noeud actuellement actif est mis en évidence.
1.  Pour vérifier ou définir la valeur d'une variable contextuelle, cliquez sur le lien **Manage context**. 

    Les variables contextuelles que vous avez éventuellement définies dans le dialogue s'affichent. 

    De plus, une variable contextuelle `$timezone` est répertoriée. L'interface utilisateur du panneau "Try it out" récupère les paramètres régionaux de l'utilisateur à partir du navigateur Web et les utilise pour définir la variable contextuelle `$timezone`. Cette variable contextuelle facilite le traitement des références temporelles dans les échanges du dialogue de test. Songez à faire la même chose dans votre application utilisateur. Si cette variable contextuelle n'est pas spécifiée, c'est l'heure moyenne de Greenwich qui est utilisée. 

    Vous pouvez ajouter une variable et définir sa valeur pour voir comment le dialogue répond lors du tour suivant du dialogue de test. Cette fonctionnalité est utile si, par exemple, le dialogue est configuré pour afficher différentes réponses en fonction d'une valeur de variable contextuelle qui est fournie par l'utilisateur. 

    1.  Pour ajouter une variable contextuelle, spécifiez le nom de la variable et appuyez sur **Entrée**.
    1.  Pour définir une valeur par défaut pour la variable contextuelle, localisez la variable contextuelle que vous avez ajoutée dans la liste, puis attribuez-lui une valeur. 

    Pour plus d'informations, reportez-vous à la section [Variables contextuelles](#context). 

1.  Continuez d'interagir avec le dialogue pour voir de quelle façon la conversation circule à travers lui. 
    - Pour rechercher et soumettre à nouveau un énoncé de test, vous pouvez appuyer sur la touche de déplacement vers le haut pour faire défiler les entrées récentes. 
    - Pour retirer les précédents énoncés de test sur le panneau de discussion et recommencer, cliquez sur le lien **Clear**. Cette action permet non seulement de retirer les énoncés et les réponses de test, mais également les valeurs des variables contextuelles qui ont été définies suite à vos interactions avec le dialogue. Les variables contextuelles que vous définissez ou modifiez explicitement ne sont pas retirées. 

### Etape suivante

Si vous constatez que les intentions ou les entités reconnues sont incorrectes, vous devrez peut-être modifier vos définitions d'intention ou d'entité. 

Si les intentions et les entités reconnues sont correctes, mais les noeuds déclenchés dans votre dialogue sont incorrects, vérifiez que vos conditions sont correctement écrites. 

## Déplacement d'un noeud de dialogue
{: #move-node}

Chaque noeud que vous créez peut être déplacé ailleurs dans l'arborescence de dialogue. 

Vous souhaiterez peut-être déplacer un noeud précédemment créé vers une autre zone du flux dans le modifier la conversation. Vous pouvez déplacer des noeuds pour qu'ils deviennent des éléments apparentés ou des homologues dans une autre branche. 

1.  Sur le noeud que vous souhaitez déplacer, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png), puis sélectionnez **Move**.
1.  Sélectionnez un noeud cible dans l'arborescence près de l'emplacement où vous souhaitez déplacer ce noeud. Indiquez si vous souhaitez placer ce noeud au-dessus ou au-dessous du noeud cible ou en faire un enfant du noeud cible. 

## Recherche d'un noeud de dialogue au moyen de son ID
{: #get-node-id}

Vous souhaiterez peut-être rechercher le noeud de dialogue qui est associé à un ID de noeud connu si vous vous trouvez dans l'une des situations suivantes : 

- Vous passez en revue des journaux, et un journal fait référence à une section du dialogue au moyen de son ID de noeud. 
- Vous souhaitez mapper les ID de noeud répertoriés dans la propriété `nodes_visited` de la sortie du message d'API aux noeuds qui sont affichés dans l'arborescence de votre dialogue. 
- Un message d'erreur d'exécution de dialogue vous informe d'une erreur de syntaxe et identifie le noeud à corriger par son ID. 

Pour rechercher un noeud à partir de son ID, procédez comme suit :

1.  A partir de l'onglet Dialog de l'outil, sélectionnez n'importe quel noeud dans l'arborescence de votre dialogue. 
1.  Fermez la vue d'édition si elle est ouverte pour le noeud en cours. 
1.  Dans la zone d'emplacement du navigateur Web, une URL doit s'afficher avec la syntaxe suivante :

    ```json
    https://watson-conversation.ng.bluemix.net/space/instance-id/workspaces/workspace-id/build/dialog#node=node-id
    ```

1.  Editez l'URL en remplaçant la valeur `node-id` en cours par l'ID du noeud que vous recherchez, puis soumettez la nouvelle URL. 
1.  Si nécessaire, mettez en évidence l'URL ainsi éditée, puis soumettez-la à nouveau. 

L'outil effectue une actualisation et le noeud de dialogue portant l'ID de noeud que vous avez spécifié est mis en évidence. 

**Remarque** : pour l'instant, vous ne pouvez pas utiliser cette méthode pour rechercher un attribut, un gestionnaire d'attributs ou un gestionnaire de niveau noeud. Pour rechercher ces types de noeud en utilisant leur ID, vous devez exporter l'espace de travail, utiliser un éditeur JSON pour localiser l'élément node-id dans le fichier JSON et noter son titre (s'il a été spécifié) ou sa condition. A partir de l'onglet Dialog de l'outil, utilisez la fonction de recherche du navigateur pour rechercher le noeud de dialogue ayant ce titre ou cette condition. 
