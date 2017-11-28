---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-08"

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

# Méthodes de traitement de valeurs

Utilisez les méthodes décrites ci-après pour traiter les valeurs extraites des énoncés d'utilisateur que vous souhaitez référencer dans une variable contextuelle, une condition, ou ailleurs dans la réponse.
{: shortdesc}

>**Remarque :** pour les méthodes impliquant des expressions régulières, reportez-vous à l'article  [RE2 Syntax reference](https://github.com/google/re2/wiki/Syntax) qui contient des informations détaillées sur la syntaxe à utiliser lorsque vous spécifiez l'expression régulière. 

## Tableaux
{: #arrays}

Vous ne pouvez pas utiliser les méthodes ci-après pour rechercher une valeur dans un tableau dans une condition de noeud ou une condition de réponse au sein du noeud dans lequel vous définissez les valeurs de tableau. 

### JSONArray.append(object)

Cette méthode ajoute une nouvelle valeur à l'élément JSONArray et renvoie l'élément JSONArray modifié.

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
{: screen}

### JSONArray.contains(object value)

Cette méthode renvoie la valeur true si l'élément JSONArray d'entrée contient la valeur d'entrée. 

Pour le contexte d'exécution de dialogue suivant, qui est défini par un noeud précédent ou une application externe :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Condition de noeud ou de réponse de dialogue :

```json
$toppings_array.contains('ham')
```
{: codeblock}

Résultat : `True`, car le tableau contient l'élément ham. 

### JSONArray.get(integer)

Cette méthode renvoie un index d'entrée à partir de l'élément JSONArray. 

Pour le contexte d'exécution de dialogue suivant, qui est défini par un noeud précédent ou une application externe :

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Condition de noeud ou de réponse de dialogue :

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Résultat : `True`, car le tableau imbriqué contient `one` comme valeur. 

Réponse :

```json
"output": {
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

Cette méthode renvoie un élément aléatoire à partir de l'élément JSONArray d'entrée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Résultat : `"ham is a great choice!"` ou `"onion is a great choice!"` ou `"olives is a great choice!"`

**Remarque :** le texte de sortie résultant est choisi de manière aléatoire.

### JSONArray.join(string delimiter)

Cette méthode joint toutes les valeurs de ce tableau à une chaîne. Les valeurs sont converties en chaîne et délimitées par le délimiteur d'entrée. 

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(integer)

Cette méthode retire l'élément de la position d'index dans l'élément JSONArray et renvoie l'élément JSONArray ainsi mis à jour.

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
{: screen}

### JSONArray.removeValue(object)

Cette méthode retire la première occurrence de la valeur dans l'élément JSONArray et renvoie l'élément JSONArray ainsi mis à jour.

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
{: screen}

### JSONArray.set(integer index, object value)

Cette méthode affecte la valeur d'entrée à l'index d'entrée de l'élément JSONArray et renvoie l'élément JSONArray ainsi modifié.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

Cette méthode renvoie la taille de l'élément JSONArray sous la forme d'un entier. 

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
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(expression régulière de type Chaîne)

Cette méthode fractionne la chaîne d'entrée à l'aide de l'expression régulière d'entrée. Le résultat obtenu est un élément JSONArray composée de chaînes. 

Pour l'entrée suivante :

```
"bananas;apples;pears"
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## Date et heure 
{: #date-time}

Plusieurs méthodes sont disponibles pour les dates et les heures. 

### .after(date-heure de type Chaîne)

- Détermine si la valeur date-heure figure après l'argument date-heure. 
- Semblable à `.before()`.

### .before(date-heure de type Chaîne)

- Par exemple :
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Détermine si la valeur date-heure figure avant l'argument date-heure. 
- Si elle compare différents éléments, par exemple, `time vs. date`, `date vs. time` et `time vs. date and time`, la méthode renvoie la valeur false et une exception est générée dans le journal JSON des réponses, `output.log_messages`.
    - Par exemple, `@sys-date.before(@sys-time)`.
- Si elle compare `date and time vs. time`, la méthode ignore la date et compare uniquement les heures. 

### now()

- Fonction statique.
- Renvoie une chaîne avec la date et l'heure en cours au format `aaaa-MM-jj HH:mm:ss`.
- Les autres méthodes dates-heure peuvent être appelées sur les valeurs date-heure qui sont renvoyées par cette fonction et peuvent être transmises en tant qu'arguments.
- Si la variable contextuelle `$timezone` est définie, cette fonction renvoie des dates et des heures exprimées dans le fuseau horaire du client. 

Exemple de noeud de dialogue avec `now()` utilisé dans la zone de sortie :

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Exemple de `now()` dans des conditions de noeud (pour décider si c'est encore le matin) :

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

Met en forme les chaînes de date et d'heure au format souhaité pour la sortie utilisateur. 

Renvoie une chaîne mise en forme selon le format spécifié :

- `MM/jj/aaaa` pour 12/31/2016
- `h a` pour 10pm

Pour renvoyer le jour de la semaine :

- `E` pour Tuesday
- `u` pour l'index de jour (1 = Monday, ..., 7 = Sunday)

Par exemple, cette définition de variable contextuelle crée une variable $time qui sauvegarde la valeur 17:30:00 sous *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Le format applique les règles Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html). 

>Remarque : lorsque le formatage porte uniquement sur l'heure, la date est interprétée comme `1970-01-01`.

### .sameMoment(String date/time)

- Détermine si la valeur date-heure est identique à l'argument date-heure. 

### .sameOrAfter(String date/time)

- Détermine si la valeur date-heure figure après ou est identique à l'argument date-heure. 
- Semblable à `.after()`.

### .sameOrBefore(String date/time)

- Détermine si la valeur date-heure figure avant ou est identique à l'argument date-heure. 

Pour plus d'informations sur les entités de système qui extraient des valeurs de date et d'heure, reportez-vous à la rubrique [Entités @sys-date et @sys-time](system-entities.html#sys-datetime).

## Nombres
{: #numbers}

### Random()

Renvoie un nombre aléatoire. Utilisez l'une des options de syntaxe suivantes :

- Pour renvoyer une valeur booléenne aléatoire (true ou false), utilisez `<?new Random().nextBoolean()?>`.
- Pour renvoyer un nombre double aléatoire compris entre 0 (inclus) et 1 (exclus), utilisez `<?new Random().nextDouble()?>`
- Pour renvoyer un nombre entier aléatoire compris entre 0 (inclus) et un nombre que vous spécifiez, utilisez `<?new Random().nextInt(n)?>`, où n est la limite supérieure de la plage numérique souhaitée + 1.
  Par exemple, si vous voulez renvoyer un nombre aléatoire compris entre 0 et 10, spécifiez `<?new Random().nextInt(11)?>`.
- Pour renvoyer un nombre entier aléatoire à partir de la plage de valeurs entières (-2147483648 à 2147483648), utilisez `<?new Random().nextInt()?>`.

Par exemple, vous pouvez créer un noeud de dialogue qui est déclenché par l'intention #random_number. La première condition de réponse peut se présenter comme suit :

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

### toDouble()

  Convertit l'objet ou la zone en type Nombre double. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé. 

### toInt()

  Convertit l'objet ou la zone en type Nombre entier. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé. 

### toLong()

  Convertit l'objet ou la zone en type Nombre long. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé. 

Pour plus d'informations sur les entités de système qui extraient des nombres, reportez-vous à la rubrique [Entité @sys-number](system-entities.html#sys-number).

## Objets
{: #objects}

### JSONObject.has(string)

Cette méthode renvoie la valeur true si l'élément JSONObject complexe contient une propriété du nom d'entrée. 

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Résultat : la condition est true car l'objet utilisateur contient la propriété `first_name`.

### JSONObject.remove(string)

Cette méthode retire une propriété du nom de l'élément `JSONObject` d'entrée. L'élément `JSONElement` qui est renvoyé par cette méthode est l'élément `JSONElement` en cours de retrait. 

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: screen}

## Chaînes
{: #strings}

### String.append(object)

Cette méthode ajoute un objet d'entrée à la chaîne sous forme de chaîne et renvoie une chaîne modifiée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

Cette méthode renvoie la valeur true si la chaîne contient la sous-chaîne d'entrée. 

Entrée : "Yes, I'd like to go."

La syntaxe suivante :

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.endsWith(string)

Cette méthode renvoie la valeur true si la chaîne se termine par la sous-chaîne d'entrée. 

Pour l'entrée suivante :

```
"What is your name?".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.extract(String regexp, Integer groupIndex)

Cette méthode renvoie une chaîne extraite par index de groupe spécifié de l'expression régulière d'entrée.

Pour l'entrée suivante :

```
"Hello 123456".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Important :** pour que `\\d` soit interprété comme une expression régulière, vous devez mettre en échappement les deux barres obliques inversées en ajoutant `\\` : `\\\\d`

Résultat :

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

Cette méthode renvoie la valeur true si l'un des segments de la chaîne correspond à l'expression régulière d'entrée. Vous pouvez appeler cette méthode sur un élément JSONArray ou JSONObject, le tableau ou l'objet sera converti en chaîne avant la comparaison. 

Pour l'entrée suivante :

```
"Hello 123456".
```
{: screen}

La syntaxe suivante :

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Résultat : la condition est true car la partie numérique du texte d'entrée correspond à l'expression régulière `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Cette méthode renvoie la valeur true si la chaîne est une chaîne vide, mais pas null. 

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.length()

Cette méthode renvoie le nombre de caractères de la chaîne. 

Pour l'entrée suivante :

```
"Hello"
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

Cette méthode renvoie la valeur true si la chaîne correspond à l'expression régulière d'entrée. 

Pour l'entrée suivante :

```
"Hello".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Résultat : la condition est true car le texte d'entrée correspond à l'expression régulière `\^Hello\$`.

### String.startsWith(string)

Cette méthode renvoie la valeur true si la chaîne débute par la sous-chaîne d'entrée. 

Pour l'entrée suivante :

```
"What is your name?".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.substring(int beginIndex, int endIndex)

Cette méthode extrait une sous-chaîne débutant par le caractère situé à `beginIndex` et se terminant par caractère défini pour l'index avant `endIndex`.
Le caractère endIndex n'est pas inclus. 

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

Cette méthode renvoie la chaîne d'origine convertie en lettres minuscules. 

Pour l'entrée suivante :

```
"This is A DOG!"
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

Cette méthode renvoie la chaîne d'origine convertie en lettres majuscules. 

Pour l'entrée suivante :

```
"hi there".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

Cette méthode enlève les espaces au début et à la fin de la chaîne et renvoie la chaîne ainsi modifiée. 

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

Pour plus d'informations sur les entités de système qui extraient des chaînes, reportez-vous à la rubrique [Entités de système](system-entities.html).
