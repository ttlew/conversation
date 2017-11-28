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

# Métodos para processar valores

Use esses métodos para processar valores extraídos de elocuções do usuário que você deseja referenciar em uma variável de contexto, condição ou em outro lugar na resposta.
{: shortdesc}

>**Nota:** para métodos que envolvem expressões regulares, consulte [Referência da Sintaxe RE2](https://github.com/google/re2/wiki/Syntax) para obter detalhes sobre a sintaxe a ser usada ao especificar a expressão regular.

## Matrizes
{: #arrays}

Não é possível usar esses métodos para procurar um valor em uma matriz em uma condição de nó ou condição de resposta dentro do mesmo nó no qual você configura os valores de matriz.

### JSONArray.append(object)

Esse método anexa um novo valor no JSONArray e retorna o JSONArray modificado.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Faça essa atualização:

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

Esse método retorna true se o JSONArray de entrada contém o valor de entrada.

Para esse Contexto de tempo de execução de diálogo que foi configurado por um nó ou aplicativo externo prévio:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Nó de diálogo ou condição de resposta:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Resultado: `True` porque a matriz contém o elemento ham.

### JSONArray.get(integer)

Esse método retorna o índice de entrada do JSONArray.

Para esse Contexto de tempo de execução de diálogo que foi configurado por um nó ou aplicativo externo prévio:

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

Nó de diálogo ou condição de resposta:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Resultado:
`True` porque a matriz aninhada contém `one` como um valor.

Resposta:

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

Esse método retorna um item aleatório do JSONArray de entrada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Saída do nó de diálogo:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Resultado: `"ham is a great choice!"` ou `"onion is a great choice!"` ou `"olives is a great choice!"`

**Nota:** O texto de saída resultante é escolhido aleatoriamente.

### JSONArray.join(string delimiter)

Esse método reúne todos os valores nessa matriz para uma sequência. Os valores são convertidos em sequência e delimitados pelo delimitador de entrada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Saída do nó de diálogo:

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

Esse método remove o elemento na posição de índice do JSONArray e retorna o JSONArray atualizado.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Faça essa atualização:

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

Esse método remove a primeira ocorrência do valor do JSONArray e retorna o JSONArray atualizado.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Faça essa atualização:

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

Esse método configura o índice de entrada do JSONArray para o valor de entrada e retorna o JSONArray modificado.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Saída do nó de diálogo:

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

Esse método retorna o tamanho do JSONArray como um número inteiro.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Faça essa atualização:

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

Esse método divide a sequência de entrada usando a expressão regular de entrada. O resultado é um JSONArray de sequências.

Para essa entrada:

```
"bananas;apples;pears"
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## Data e Hora
{: #date-time}

Vários métodos estão disponíveis para trabalhar com data e hora.

### .after(String date/time)

- Determina se o valor de data/hora é após o argumento de data/hora.
- Análogo a `.before()`.

### .before(String date or time)

- Por exemplo:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Determina se o valor de data/hora é anterior ao argumento de data/hora.
- Se comparar itens diferentes, como `time vs. date`, `date vs. time` e `time vs. date and time`, o método retornará falso e uma exceção será impressa no log JSON de resposta `output.log_messages`.
    - Por exemplo, `@sys-date.before(@sys-time)`.
- Se comparar `date and time vs. time` o método ignorará a data e comparará apenas os horários.

### now()

- Função estática.
- Retorna uma sequência com a data e hora atuais no formato `yyyy-MM-dd HH:mm:ss`.
- Os outros métodos de data/hora podem ser chamados em valores de data/hora que são retornados por essa função e ela pode ser transmitida como seus argumentos.
- Se a variável de contexto `$timezone` estiver configurada, essa função retornará datas e horas no fuso horário do cliente.

Exemplo de um nó de diálogo com `now()` usado no campo de saída:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Exemplo de `now()` em condições do nó (para decidir se ainda é de manhã):

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

Formata sequências de data e hora no formato desejado para a saída de usuário.

Retorna uma sequência formatada de acordo com o formato especificado:

- `MM/dd/yyyy` para 12/31/2016
- `h a` para 10pm

Para retornar o dia da semana:

- `E` para terça-feira
- `u` para o índice do dia (1 = Segunda..,, 7 = Domingo)

Por exemplo, essa definição de variável de contexto cria uma variável $time que salva o valor 17:30:00 como *17:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

O formato segue as regras [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) de Java.

>Nota: ao tentar formatar apenas o horário, a data é tratada como `1970-01-01`.

### .sameMoment(String date/time)

- Determina se o valor de data/hora é o mesmo que o argumento de data/hora.

### .sameOrAfter(String date/time)

- Determina se o valor de data/hora é posterior ou igual ao argumento de data/hora.
- Análogo a `.after()`.

### .sameOrBefore(String date/time)

- Determina se o valor de data/hora é anterior ou igual ao argumento de data/hora.

Para obter informações sobre entidades do sistema que extraem valores de data e hora, consulte [Entidades @sys-date e @sys-time](system-entities.html#sys-datetime).

## Números
{: #numbers}

### Random()

Retorna um número aleatório. Use uma das opções de sintaxe a seguir:

- Para retornar um valor booleano aleatório (true ou false), use `<?new Random().nextBoolean()?>`.
- Para retornar um número duplo aleatório entre 0 (incluído) e 1 (excluído), use `<?new Random().nextDouble()?>`
- Para retornar um número inteiro aleatório entre 0 (incluído) e um número que você especificar, use `<?new Random().nextInt(n)?>` em que n é o topo do intervalo de números que deseja + 1.
  Por exemplo, se você deseja retornar um número aleatório entre 0 e 10, especifique `<?new Random().nextInt(11)?>`.
- Para retornar um número inteiro aleatório do intervalo de valores de número inteiro completo (-2147483648 a 2147483648), use `<?new Random().nextInt()?>`.

Por exemplo, você pode criar um nó de diálogo que é acionado pela intenção #random_number. A primeira condição de resposta pode ser semelhante a esta:

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

  Converte o objeto ou campo no tipo de número Duplo. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

### toInt()

  Converte o objeto ou campo no tipo de número Inteiro. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

### toLong()

  Converte o objeto ou campo no tipo de número Longo. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

Para obter informações sobre entidades do sistema que extraem números, consulte [Entidade @sys-number](system-entities.html#sys-number).

## Objetos
{: #objects}

### JSONObject.has(string)

Esse método retorna true se o JSONObject complexo possui uma propriedade do nome de entrada.

Para esse Contexto de tempo de execução de diálogo:

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

Saída do nó de diálogo:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Resultado: a condição é true porque o objeto de usuário contém a propriedade `first_name`.

### JSONObject.remove(string)

Esse método remove uma propriedade do nome da entrada `JSONObject`. O `JSONElement` que é retornado por esse método é o `JSONElement` que está sendo removido.

Para esse Contexto de tempo de execução de diálogo:

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

Saída do nó de diálogo:

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

## Sequências
{: #strings}

### String.append(object)

Esse método anexa um objeto de entrada à sequência como uma sequência e retorna uma sequência modificada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

Esse método retorna true se a sequência contém a subsequência de entrada.

Entrada: "Yes, I'd like to go."

Essa sintaxe:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.endsWith(string)

Esse método retorna true se a sequência termina com a subsequência de entrada.

Para essa entrada:

```
"What is your name?".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.extract(String regexp, Integer groupIndex)

Esse método retorna uma sequência extraída pelo índice de grupo especificado da expressão regular de entrada.

Para essa entrada:

```
"Hello 123456".
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Importante:** para processar `\\d` como a expressão regular, você precisa escapar as barras invertidas incluindo outro `\\`: `\\\\d`

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

Esse método retorna true se qualquer segmento da sequência corresponde à expressão regular de entrada. É possível chamar esse método em um elemento JSONArray ou JSONObject, e ele converterá a matriz ou o objeto em uma sequência antes de fazer a comparação.

Para essa entrada:

```
"Hello 123456".
```
{: screen}

Essa sintaxe:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Resultado: a condição é true porque a parte numérica do texto de entrada corresponde à expressão regular `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Esse método retorna true se a sequência é uma sequência vazia, mas não nula.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.length()

Esse método retorna o comprimento de caracteres da sequência.

Para essa entrada:

```
"Hello"
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

Esse método retorna true se a sequência corresponde à expressão regular de entrada.

Para essa entrada:

```
"Hello".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Resultado: a condição é true porque o texto de entrada corresponde à expressão regular `\^Hello\$`.

### String.startsWith(string)

Esse método retorna true se a sequência inicia com a subsequência de entrada.

Para essa entrada:

```
"What is your name?".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.substring(int beginIndex, int endIndex)

Esse método obtém uma subsequência com o caractere em `beginIndex` e o último caractere configurado como índice antes de `endIndex`.
O caractere endIndex não está incluído.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

Esse método retorna a Sequência original convertida em letras minúsculas.

Para essa entrada:

```
"This is A DOG!"
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

Esse método retorna a Sequência original convertida em letras maiúsculas.

Para essa entrada:

```
"hi there".
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

Esse método reduz quaisquer espaços no início e no final da sequência e retorna a sequência modificada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

Para obter informações sobre entidades do sistema que extraem sequências, consulte [Entidades do Sistema](system-entities.html).
