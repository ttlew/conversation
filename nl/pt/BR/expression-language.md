---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-15"

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

# Acessando e avaliando objetos

Expressões válidas em condições são gravadas no idioma Spring Expression (SpEL). Para obter informações adicionais, consulte [Spring Expression Language (SpEL) language ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Variáveis globais integradas

As variáveis globais a seguir estão disponíveis:

| Variável global      | Definição  |
|----------------------|------------|
| *anything_else*      | O último nó do diálogo inteiro. Quando a entrada do usuário não pode ser correspondida a uma intenção, esse nó é executado. |
| *context*            | Parte de objeto JSON da mensagem de conversa processada. |
| *conversation_start* | Um valor booleano que é verdadeiro na primeira rodada de conversas do diálogo (pode ser usado em uma condição de um nó de diálogo para definir uma mensagem de boas-vindas no diálogo). |
| *entities[ ]*        | Lista de entidades que suportam acesso padrão ao primeiro elemento. |
| *input*              | Parte de objeto JSON da mensagem de conversa processada. |
| *intents[ ]*         | Lista de intenções que suportam acesso padrão ao primeiro elemento. |
| *output*             | Parte de objeto JSON da mensagem de conversa processada. |

## Acessando entidades

A matriz de entidades contém uma ou mais entidades.

Ao testar seu diálogo, é possível ver detalhes das entidades que são reconhecidas na entrada do usuário especificando essa expressão em uma resposta do nó de diálogo:

```json
<? entities ?>
```
{: codeblock}

Para a entrada do usuário *Hello now*, o serviço reconhece as entidades de sistema @sys-date e @sys-time, portanto a resposta contém esses objetos de entidade:

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Sintaxe abreviada para entidades

A tabela a seguir mostra exemplos de sintaxe abreviada que podem ser usados ao se referir a entidades.

| Sintaxe de abreviação   | Sintaxe completa em SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

Em SpEL, o ponto de interrogação `(?)` evita que uma exceção de ponteiro nulo seja acionada quando um objeto de entidade for nulo.

Se o valor da entidade que você deseja verificar contiver um caractere `)`, não será possível usar o operador `:` para comparação.  Por exemplo, se desejar verificar se a entidade city é `Dublin (Ohio)`, você deverá usar `@city == 'Dublin (Ohio)'`
em vez de `@city:(Dublin (Ohio))`.

### Quando o posicionamento de entidades na entrada importa

Use a expressão SpEL completa se o posicionamento de entidades na entrada importar. A condição `entities['city']?.contains('Boston')` retorna verdadeiro quando pelo menos uma entidade city 'Boston' é encontrada em todas as entidades @city, independentemente do posicionamento.

Por exemplo, um usuário envia `"I want to go from Toronto to Boston."`. As entidades `@city:Toronto` e `@city:Boston` são detectadas e representadas nessas entidades:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

A condição `@city.contains('Boston')` em um nó de diálogo retorna verdadeiro apesar de Boston ser a segunda entidade detectada.

### Propriedades da entidade

Cada entidade possui um conjunto de propriedades associadas a ela. É possível acessar informações sobre uma entidade através de suas propriedades.

| Propriedade           | Definição  | Dicas de Uso |
|-----------------------|------------|------------|
| *confiança*          | Uma porcentagem decimal que representa a confiança do serviço na entidade reconhecida. A confiança de uma entidade é 0 ou 1, a menos que você tenha ativado a correspondência difusa de entidades. Quando a correspondência difusa está ativada, o limite de nível de confiança padrão é 0,3. Independentemente de a correspondência difusa estar ativada ou não, as entidades do sistema sempre terão um nível de confiança 1,0. | Será possível usar essa propriedade em uma condição para que ela retorne falso se o nível de confiança não for maior que um percentual especificado. |
| *localização*            | Um deslocamento de caractere baseado em zero que indica onde os valores de entidade detectados começam e terminam no texto de entrada. | Use `.literal` para extrair o período de texto entre os valores de índice iniciais e finais que estão armazenados na propriedade localização. |
| *valor*               | O valor da entidade identificado na entrada. | Essa propriedade retorna o valor da entidade conforme definido nos dados de treinamento, mesmo se a correspondência foi feita contra um de seus sinônimos associados. É possível usar `.values` para capturar várias ocorrências de uma entidade que podem estar presentes na entrada do usuário. |

### Exemplos de uso da propriedade da entidade
Nos exemplos a seguir, a área de trabalho contém uma entidade aeroporto que inclui um valor de JFK e o sinônimo "Kennedy Airport". A entrada do usuário é *Eu quero ir para o aeroporto Kennedy*.

- Para retornar uma resposta específica se a entidade 'JFK' for reconhecida na entrada do usuário, você poderia incluir essa expressão para a condição de resposta: `entities.airport[0].value == 'JFK'` ou `@airport = "JFK"`
- Para retornar o nome da entidade como foi especificado pelo usuário na resposta do diálogo, use a propriedade .literal:
`So you want to go to <?entities.airport[0].literal?>...`
  ou
  `So you want to go to @airport.literal ...`

  Ambos os formatos são avaliados para `So you want to go to Kennedy Airport...' na resposta.

- Expressões como `@airport:(JFK)` ou `@airport.contains('JFK')` sempre referem-se ao **valor** da entidade (`JFK` neste exemplo).
- Para ser mais restritivo sobre quais termos são identificados como aeroportos na entrada quando a correspondência difusa estiver ativada, é possível especificar essa expressão em uma condição de nó, por exemplo: `@airport && @airport.confidence > 0.7`. O nó executará apenas se o serviço estiver 70% confiante de que o texto de entrada contém uma referência ao aeroporto.

Nesse exemplo, a entrada do usuário é *Há lugares para troca de moeda no JFK, Logan e O'Hare?*

- Para capturar diversas ocorrências de um tipo de entidade na entrada do usuário, use uma sintaxe como esta:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Para referir-se posteriormente à lista capturada em uma resposta de diálogo, use esta sintaxe:
`You asked about these airports: <? $airports.join(', ') ?>.`
  Ela é exibida assim:
`You asked about these airports: JFK, Logan, O'Hare.`

## Acessando intenções

A matriz de intenções contém uma ou mais intenções que foram reconhecidas na entrada do usuário, classificadas em ordem decrescente de confiança. Cada intenção tem apenas uma propriedade: a propriedade confiança. A propriedade confiança é uma porcentagem decimal que representa a confiança do serviço na intenção reconhecida.

Ao testar seu diálogo, é possível ver detalhes das intenções que são reconhecidas na entrada do usuário, especificando essa expressão em uma resposta do nó diálogo:

```json
<? intents ?>
```
{: codeblock}

Para a entrada do usuário *Hello now*, o serviço está confiante de que a intenção #greeting é a melhor correspondência para a entrada do usuário, portanto lista os detalhes do objeto da intenção #greeting primeiro. A resposta também inclui todas as outras intenções que estão definidas no espaço de trabalho, mesmo que sua confiança nelas esteja tão baixa que seja arredondada para 0.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

Os exemplos a seguir mostram como verificar um valor de intenção:

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help` difere de `intents[0] == 'help'` porque `intent == 'help'` não lança uma exceção se nenhuma intenção for detectada. Ela é avaliada como verdadeira somente se a confiança na intenção exceder um limite. Se você quiser, será possível especificar um nível de confiança customizado para uma condição, por exemplo, `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

### Sintaxe abreviada para intenções

A tabela a seguir mostra exemplos de sintaxe abreviada que você pode usar ao se referir a intenções.

| Sintaxe de abreviação       | Sintaxe completa em SpEL |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` or `#i_am_lost` | <code>(intent == 'help' &#124;&#124; intent == 'I_am_lost')</code> |

## Acessando a entrada

O objeto JSON de entrada contém uma única propriedade: a propriedade de texto. A propriedade de texto representa o texto da entrada do usuário.

### Exemplos de uso da propriedade de entrada

O exemplo a seguir mostra como acessar a entrada:

- Para executar um nó se a entrada do usuário for "Sim", inclua essa expressão para a condição nó:
`input.text == 'Yes'`

É possível usar qualquer [Método de sequência](/docs/services/conversation/dialog-methods.html#strings) para avaliar ou manipular texto da entrada do usuário. Por exemplo:

- Para verificar se a entrada do usuário contém "Yes", use: `input.text.contains( 'Yes' )`.
- Retorna verdadeiro se a entrada do usuário for um número: `input.text.matches( '[0-9]+' )`.
- Para verificar se a cadeia de entrada contém dez caracteres, use: `input.text.length() == 10`.

## Sintaxe abreviada para variáveis de contexto

A tabela a seguir mostra exemplos da sintaxe abreviada que você pode utilizar para gravar variáveis de contexto em expressões de condição.

| Sintaxe de abreviação           | Sintaxe integral em SpEL                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

É possível incluir caracteres especiais como hifens ou pontos em nomes de variável de contexto. No entanto, fazer isso pode levar a problemas quando a expressão SpEL é avaliada. O hífen poderia ser interpretado como um sinal de menos, por exemplo. Para evitar tais problemas, referencie a variável usando a sintaxe de expressão completa ou a sintaxe abreviada `$(variable-name)` e não use os caracteres especiais a seguir no nome:

- Parênteses ()
- Mais de um apóstrofo ''
- Aspas "

## Avaliação

Para expandir os valores de variáveis dentro de outras variáveis ou aplicar métodos para variáveis, use a sintaxe de expressão `<? expression ?>`. Por exemplo:

- **Expandindo uma propriedade**
    - `"output":{"text":"Your name is <? context.userName ?>"}`
    ou
    - `"output":{"text":"Your name is $userName"}` em sintaxe abreviada
- **Incrementando uma propriedade numérica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Chamando métodos como append em propriedades e objetos globais**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

### Objeto ou Formato de sequência JSON

Ao usar a sintaxe SpEL completa na resposta do diálogo, coloque a expressão em `<?` e `?>` para que seja renderizada no formato de sequência. Ao usar a sintaxe SpEL completa em uma condição, não inclua a sintaxe `<? ?>` circundante.

Se você especificar uma expressão SpEL em uma condição, as informações serão retornadas em formato de objeto para que o valor resultante possa reter o seu tipo de dados e ser usado em equações ou outras expressões. Se você especificar mais de uma expressão SpEL em uma condição ou incluir a expressão como parte de uma sequência, então as informações serão retornadas no formato de sequência ao invés. 

Por exemplo, é possível incluir essa expressão em uma resposta do nó de diálogo para retornar as entidades que são reconhecidas na entrada do usuário:

```json
The entities are <? entities ?>.
```
{: codeblock}

Se o usuário especificar *Hello now* como a entrada, as informações de entidade serão fornecidas no formato de sequência.

```json
The entities are 2017-08-07, 15:09:49.
```
{: codeblock}

É possível incluir essa expressão para uma resposta do nó de diálogo para retornar as intenções que são reconhecidas na entrada do usuário:

```json
The intents are <? intents ?>.
```
{: codeblock}

Se o usuário especificar *Hello now* como a entrada, as informações da intenção serão fornecidas no formato de sequência.

```json
The intents are [
{"intent":"greeting","confidence":0.9331061244010925},
{"intent":"yes","confidence":0.06050306558609009},
{"intent":"pizza-order","confidence":0.052069634199142456},
...
]
```
{: codeblock}
