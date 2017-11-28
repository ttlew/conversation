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

# Métodos para procesar valores

Utilice estos métodos para procesar valores extraídos de expresiones del usuario a los que desea hacer referencia en una variable de contexto, condición o en alguna otra parte de la respuesta.
{: shortdesc}

>**Nota:** Para los métodos que implican expresiones regulares, consulte la [Referencia de sintaxis de RE2](https://github.com/google/re2/wiki/Syntax) para ver detalles sobre la sintaxis que se debe utilizar cuando se especifica la expresión regular.

## Matrices
{: #arrays}

No puede utilizar estos métodos para comprobar un valor de una matriz en una condición de nodo o condición de respuesta dentro del mismo nodo en el que establece los valores de matriz.

### JSONArray.append(object)

Este método añade un nuevo valor a JSONArray y devuelve la matriz JSONArray modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: screen}

### JSONArray.contains(object value)

Este método devuelve true si la matriz JSONArray de entrada contiene el valor de entrada.

Para este contexto de tiempo de ejecución del diálogo establecido por un nodo anterior o aplicación externa:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Nodo de diálogo o condición de respuesta:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Resultado: `True` porque la matriz contiene el elemento ham.

### JSONArray.get(integer)

Este método devuelve el índice de entrada de la matriz JSONArray.

Para este contexto de tiempo de ejecución del diálogo establecido por un nodo anterior o aplicación externa:

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

Nodo de diálogo o condición de respuesta:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Resultado: `True` porque la matriz anidada contiene `one` como valor.

Respuesta:

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

Este método devuelve un elemento aleatorio procedente de la JSONArray de entrada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Resultado: `"ham is a great choice!"` o bien `"onion is a great choice!"` o bien `"olives is a great choice!"`

**Nota:** El texto de salida resultante se elige aleatoriamente.

### JSONArray.join(string delimiter)

Este método une todos los valores de esta matriz a una serie. Los valores se convierten en una serie y se delimitan mediante el delimitador de entrada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(integer)

Este método elimina el elemento en la posición de índice de JSONArray y devuelve la matriz JSONArray actualizada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.removeValue(object)

Este método elimina la primera aparición del valor de JSONArray y devuelve la matriz JSONArray actualizada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.set(integer index, object value)

Este método establece el índice de entrada de JSONArray en el valor de entrada y devuelve la matriz JSONArray modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

Este método devuelve el tamaño de la matriz JSONArray como un entero.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(String regexp)

Este método divide la serie de entrada utilizando la expresión regular de entrada. El resultado es una matriz JSONArray de series.

Por esta entrada:

```
"bananas;apples;pears"
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## Fecha y hora
{: #date-time}

Dispone de varios métodos para trabajar con datos de fecha y hora.

### .after(String date/time)

- Determina si el valor de fecha y hora es posterior al argumento date/time.
- Similar a `.before()`.

### .before(String date or time)

- Por ejemplo:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Determina si el valor de fecha y hora es anterior al argumento date/time.
- Si se comparan distintos elementos, como por ejemplo `time vs. date`, `date vs. time` y `time vs. date and time`, el método devuelve false y se muestra una excepción en el registro JSON de respuestas `output.log_messages`.
    - Por ejemplo, `@sys-date.before(@sys-time)`.
- Si se compara `date and time vs. time`, el método pasa por alto la fecha y solo compara las horas.

### now()

- Función estática.
- Devuelve una serie con la fecha y hora actuales en el formato `aaaa-MM-dd HH:mm:ss`.
- Los otros métodos de fecha y hora se pueden invocar en valores de fecha y hora que devuelve esta función y se pueden pasar como argumento.
- Si se ha definido la variable de contexto `$timezone`, esta función devuelve fechas y horas en el huso horario del cliente.

Ejemplo de un nodo de diálogo en el que se utiliza `now()` en el campo de salida:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Ejemplo de `now()` en condiciones del nodo (para decidir si aún es por la mañana):

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

Formatea las series de fecha y hora con el formato deseado para la salida de usuario.

Devuelve una serie formateada según el formato especificado:

- `MM/dd/aaaa` para 12/31/2016
- `h a` para 10pm

Para devolver el día de la semana:

- `E` para martes
- `u` para índice de días (1 = lunes, ..., 7 = domingo)

Por ejemplo, esta definición variable de contexto crea una variable $time que guarda el valor 17:30:00 como *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

El formato sigue las reglas [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) de Java.

>Nota: Cuando se intenta formatear solo la hora, la fecha se trata como `1970-01-01`.

### .sameMoment(String date/time)

- Determina si el valor de fecha y hora es igual al argumento date/time.

### .sameOrAfter(String date/time)

- Determina si el valor de fecha y hora es posterior o igual al argumento date/time.
- Similar a `.after()`.

### .sameOrBefore(String date/time)

- Determina si el valor de fecha y hora es anterior o igual al argumento date/time.

Para obtener información sobre las entidades del sistema que extraen valores de fecha y hora, consulte las [entidades @sys-date y @sys-time](system-entities.html#sys-datetime).

## Números
{: #numbers}

### Random()

Devuelve un número aleatorio. Utilice una de las siguientes opciones de sintaxis:

- Para devolver un valor booleano aleatorio (true o false), utilice `<?new Random().nextBoolean()?>`.
- Para devolver un número doble aleatorio comprendido entre 0 (incluido) y 1 (excluido), utilice `<?new Random().nextDouble()?>`
- Para devolver un entero aleatorio comprendido entre 0 (incluido) y el número que especifique, utilice `<?new Random().nextInt(n)?>`  donde n es el límite superior del rango de números que desea + 1. 
  Por ejemplo, si desea que se devuelva un número aleatorio entre comprendido entre 0 y 10, especifique `<?new Random().nextInt(11)?>`.
- Para devolver un entero aleatorio del rango completo de valores enteros (entre -2147483648 y 2147483648), utilice `<?new Random().nextInt()?>`.

Por ejemplo, supongamos que desea crear un nodo de diálogo que se activa mediante la intención #random_number. La primera condición de respuesta se parecería a la siguiente:

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

  Convierte el objeto o campo al tipo de número Double (doble). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

### toInt()

  Convierte el objeto o campo al tipo de número Integer (entero). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

### toLong()

  Convierte el objeto o campo al tipo de número Long (largo). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

Para obtener información sobre las entidades del sistema que extraen números, consulte la [entidad @sys-number](system-entities.html#sys-number).

## Objetos
{: #objects}

### JSONObject.has(string)

Este método devuelve true si el objeto JSONObject complejo tiene una propiedad del nombre de entrada.

Para este contexto de tiempo de ejecución del diálogo:

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

Salida del nodo del diálogo:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Resultado: La condición es true (verdadera) porque el objeto de usuario contiene la propiedad `first_name`.

### JSONObject.remove(string)

Este método elimina una propiedad del nombre de la entrada `JSONObject`. El elemento `JSONElement` que devuelve este método es el `JSONElement` que se va a eliminar.

Para este contexto de tiempo de ejecución del diálogo:

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

Salida del nodo del diálogo:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Resultado:

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

## Series
{: #strings}

### String.append(object)

Este método añade un objeto de entrada a la serie como serie (string) y devuelve una serie modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

Este método devuelve true si la serie contiene la subserie de entrada.

Entrada: "Yes, I'd like to go."

Esta sintaxis:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.endsWith(string)

Este método devuelve true si la serie termina por la subserie de entrada.

Por esta entrada:

```
"What is your name?".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.extract(String regexp, Integer groupIndex)

Este método devuelve una serie extraída por el índice de grupo especificado de la expresión regular de entrada.

Por esta entrada:

```
"Hello 123456".
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Importante:** Para procesar `\\d` como expresión regular, debe añadir un escape a ambas barras inclinadas invertidas; para ello debe añadir otro `\\`: `\\\\d`

Resultado:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

Este método devuelve true si algún segmento de la serie coincide con la expresión regular de entrada.  Puede llamar a este método sobre un elemento JSONArray o JSONObject y convertirá la matriz o el objeto en una serie antes de realizar la comparación.

Por esta entrada:

```
"Hello 123456".
```
{: screen}

Esta sintaxis:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Da como resultado: La condición es true porque la parte numérica de la entrada coincide con la expresión regular `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Este método devuelve true si la serie termina es una serie vacía pero no nula.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.length()

Este método devuelve la longitud en caracteres de la serie.

Por esta entrada:

```
"Hello"
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

Este método devuelve true si la serie coincide con la expresión regular de entrada.

Por esta entrada:

```
"Hello".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Da como resultado: La condición es true porque el texto coincide con la expresión regular `\^Hello\$`.

### String.startsWith(string)

Este método devuelve true si la serie empieza por la subserie de entrada.

Por esta entrada:

```
"What is your name?".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.substring(int beginIndex, int endIndex)

Este método obtiene una subserie con el carácter de `beginIndex` y el último carácter establecido en el índice antes de `endIndex`.
El carácter endIndex no se incluye.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

Este método devuelve la serie original convertida a letras minúsculas.

Por esta entrada:

```
"This is A DOG!"
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

Este método devuelve la serie original convierten a letras mayúsculas.

Por esta entrada:

```
"hi there".
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

Este método elimina los espacios al principio y al final de la serie y devuelve la serie modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

Para obtener información sobre las entidades del sistema que extraen series, consulte [Entidades del sistema](system-entities.html).
