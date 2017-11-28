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

# 访问对象和对象求值

条件中的有效表达式是用 Spring Expression (Spel) 语言编写的。有关更多信息，请参阅 [Spring Expression Language (SpEL) 语言 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}。
{: shortdesc}

## 内置全局变量

提供了以下全局变量：

| 全局变量             | 定义       |
|----------------------|------------|
| *anything_else*      | 整个对话的最后一个节点。如果用户输入无法与意向匹配，那么将执行此节点。|
| *context*            | 已处理会话消息的 JSON 对象部分。|
| *conversation_start* | 在第一轮对话会话中为 true 的布尔值（可以用在对话节点的条件中来定义对话欢迎消息）。|
| *entities[ ]*        | 支持缺省访问第一个元素的实体的列表。|
| *input*              | 已处理会话消息的 JSON 对象部分。|
| *intents[ ]*         | 支持缺省访问第一个元素的意向的列表。|
| *output*             | 已处理会话消息的 JSON 对象部分。|

## 访问实体

实体数组包含一个或多个实体。

测试对话时，通过在对话节点响应中指定以下表达式，可以查看在用户输入中识别到的实体的详细信息：

```json
<? entities ?>
```
{: codeblock}

对于用户输入 *Hello now*，服务可识别到 @sys-date 和 @sys-time 系统实体，因此响应包含以下实体对象：

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

### 实体的简写语法

下表显示了可以在引用实体时使用的简写语法示例。

| 简写语法            | 使用 SpEL 的完整语法                     |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

在 SpEL 中，问号 `(?)` 会阻止实体对象为空时触发空指针异常。

如果要检查的实体值包含 `)` 字符，那么不能使用 `:` 运算符进行比较。例如，如果要检查 city 实体是否为 `Dublin (Ohio)`，那么必须使用 `@city == 'Dublin (Ohio)'`，而不能使用 `@city:(Dublin(Ohio))`。

### 在输入内容中放置实体

如果在输入内容中放置实体，请使用完整的 SpEL 表达式。如果在所有 @city 实体中至少有一个“Boston”city 实体，那么无论该实体放置在哪，条件 `entities['city']?.contains('Boston')` 都会返回 true。

例如，用户提交 `"I want to go from Toronto to Boston."`。在以下实体中会检测到并表示 `@city:Toronto` 和 `@city:Boston` 实体：

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

虽然 Boston 是检测到的第二个实体，但是对话节点中的 `@city.contains('Boston')` 条件仍会返回 true。

### 实体属性

每个实体都有一组与之关联的属性。可以通过实体的属性访问有关实体的信息。

| 属性                  | 定义       | 使用技巧   |
|-----------------------|------------|------------|
| *confidence*          | 表示服务对已识别实体的置信度的小数百分比。除非激活了实体模糊匹配，否则实体的置信度为 0 或 1。启用模糊匹配后，缺省置信度级别阈值为 0.3。无论是否启用模糊匹配，系统实体的置信度级别都始终为 1.0。| 可以在条件中使用此属性，在置信度级别不高于您指定的百分比时，使其返回 false。|
| *location*            | 从零开始的字符偏移量，用于指示检测到的实体值在输入文本中的起始和结束位置。| 使用 `.literal` 可抽取 location 属性中存储的起始索引值与结束索引值之间的文本范围。|
| *value*               | 在输入中识别到的实体值。| 此属性返回在培训数据中定义的实体值，即便已针对其某个关联同义词进行了匹配也不例外。可以使用 `.values` 来捕获可能在用户输入出现多次的实体。|

### 实体属性用法示例
在以下示例中，工作空间包含 airport 实体，此实体包含值 JFK 和同义词“Kennedy Airport”。用户输入为 *I want to go to Kennedy Aiport*。

- 要在用户输入中识别到“JFK”实体时返回特定响应，可以将以下表达式添加到响应条件：`entities.airport[0].value == 'JFK'` 或 `@airport = "JFK"`
- 要返回由用户在对话响应中指定的实体名称，请使用 .literal 属性：`So you want to go to <?entities.airport[0].literal?>...`
  或 `So you want to go to @airport.literal ...`

  这两种格式都会在响应中求值为“So you want to go to Kennedy Airport...”。

- 像 `@airport:(JFK)` 或 `@airport.contains('JFK')` 这样的表达式始终引用实体（在此示例中为 `JFK`）的**值**。
- 例如，要在启用模糊匹配时对输入中识别为机场的词汇进行更强的限制，可以在节点条件中指定以下表达式：`@airport && @airport.confidence > 0.7`。仅当服务对输入文本包含机场引用的置信度为 70% 时，才会执行此节点。

在此示例中，用户输入为 *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- 要捕获在用户输入中多次出现的实体类型，请使用如下语法：

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  要稍后在对话响应中引用捕获的列表，请使用以下语法：`You asked about these airports: <? $airports.join(', ') ?>.`
  显示的响应类似于：`You asked about these airports: JFK, Logan, O'Hare.`

## 访问意向

意向数组包含在用户输入中已识别到的一个或多个意向，按置信度降序排序。每个意向只有一个属性：confidence 属性。confidence 属性为小数百分比，表示服务对识别到的意向的置信度。

测试对话时，通过在对话节点响应中指定以下表达式，可以查看在用户输入中识别到的意向的详细信息：

```json
<? intents ?>
```
{: codeblock}

对于用户输入 *Hello now*，服务确信 #greeting 意向是用户输入的最佳匹配，因此首先列出 #greeting 意向对象的详细信息。响应还包含工作空间中定义的其他所有意向，即便服务对这些意向的置信度非常低，甚至可舍入为 0，也会包含在内。

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

以下示例显示如何检查意向值：

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` 不同于 `intents[0] == 'help'`，因为如果未检测到意向，`intent == 'help'` 不会抛出异常。仅当意向置信度超过阈值时，它才会求值为 true。如果需要，可以为条件指定定制置信度级别，例如 `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

### 意向的简写语法

下表显示了可以在引用意向时使用的简写语法示例。

| 简写语法                | 使用 SpEL 的完整语法|
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` 或 `#i_am_lost` | <code>(intent == 'help' &#124;&#124; intent == 'I_am_lost')</code> |

## 访问输入

输入 JSON 对象仅包含一个属性：text 属性。text 属性表示用户输入的文本。

### 输入属性用法示例

以下示例显示如何访问输入：

- 要在用户输入为“Yes”时执行节点，请向节点条件添加以下表达式：`input.text == 'Yes'`

可以使用任何[字符串方法](/docs/services/conversation/dialog-methods.html#strings)对用户输入的文本进行求值或处理。例如：

- 要检查用户输入是否包含“Yes”，请使用：`input.text.contains( 'Yes' )`。
- 如果用户输入的是数字，那么返回 true：`input.text.matches( '[0-9]+' )`。
- 要检查输入字符串是否包含 10 个字符，请使用：`input.text.length() == 10`。

## 上下文变量的简写语法

下表显示了可用于在条件表达式中编写上下文变量的简写语法示例。

| 简写语法                   | 使用 SpEL 的完整语法                    |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

可以在上下文变量名称中包含特殊字符，例如连字符或句点。但是，这样做会导致对 SpEL 表达式求值时出现问题。例如，连字符可能解释为减号。要避免此类问题，请使用完整的表达式语法或简写语法 `$(variable-name)` 来引用该变量，并且不要在名称中使用以下特殊字符：

- 圆括号 ()
- 多个单引号 ''
- 引号 "

## 求值

要在其他变量内部扩展变量值，或要将方法应用于变量，请使用 `<? expression ?>` 表达式语法。例如：

- **扩展属性**
    - `"output":{"text":"Your name is <? context.userName ?>"}` 或
    - `"output":{"text":"Your name is $userName"}`（简写语法）
- **递增数字属性**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **对属性和全局对象调用方法，例如 append 方法**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

### JSON 对象或字符串格式

在对话响应中使用完整 SpEL 语法时，请将表达式括在 `<?` 和 `?>` 之间，使其以字符串格式呈现。在条件中使用完整 SpEL 语法时，不要包含括起符号 `<? ?>` 语法。

如果在条件中指定一个 SpEL 表达式，那么将以对象格式返回信息，以便生成的值可以保留其数据类型并在公式或其他表达式中使用。如果在条件中指定多个 SpEL 表达式，或者在字符串中包含 SpEL 表达式，那么将改为以字符串格式返回信息。 

例如，可以将以下表达式添加到对话节点响应，以返回在用户输入中识别到的实体：

```json
实体为 <? entities ?>。
```
{: codeblock}

如果用户将 *Hello now* 指定为输入，那么将以字符串格式提供实体信息。

```json
实体为 2017-08-07 15:09:49。
```
{: codeblock}

可以将以下表达式添加到对话节点响应，以返回在用户输入中识别到的意向：

```json
意向为 <? intents ?>。
```
{: codeblock}

如果用户将 *Hello now* 指定为输入，那么将以字符串格式提供意向信息。

```json
意向为 [
{"intent":"greeting","confidence":0.9331061244010925},
{"intent":"yes","confidence":0.06050306558609009},
{"intent":"pizza-order","confidence":0.052069634199142456},
...
]
```
{: codeblock}
