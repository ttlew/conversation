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

# 值处理方法

使用以下方法来处理从用户发声中抽取的值，这些值将根据您的需要在上下文变量、条件或响应中的其他地方进行引用。
{: shortdesc}

>**注：**有关涉及正则表达式的方法，请参阅 [RE2 语法参考](https://github.com/google/re2/wiki/Syntax)，以了解有关指定正则表达式时使用的语法的详细信息。

## 数组
{: #arrays}

如果节点已经设置节点条件或响应条件中的数组值，那么不能使用以下方法来检查该节点中的这些数组值。

### JSONArray.append(object)

此方法用于将新值附加到 JSONArray，并返回修改后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: screen}

### JSONArray.contains(object value)

如果输入 JSONArray 包含输入值，那么此方法会返回 true。

对于由前一个节点或外部应用程序设置的以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点或响应条件：

```json
$toppings_array.contains('ham')
```
{: codeblock}

结果：`True`，因为数组包含元素 ham。

### JSONArray.get(integer)

此方法用于返回 JSONArray 中的输入索引。

对于由前一个节点或外部应用程序设置的以下对话运行时上下文：

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

对话节点或响应条件：

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

结果：`True`，因为嵌套数组包含 `one` 作为值。

响应：

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

此方法用于返回输入 JSONArray 中的随机项。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

结果：`"ham is a great choice!"`、`"onion is a great choice!"` 或 `"olives is a great choice!"`

**注：**生成的输出文本是随机选择的。

### JSONArray.join(string delimiter)

此方法用于将此数组中的所有值连接成一个字符串。值将转换为字符串，并用输入定界符进行定界。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

结果：

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(integer)

此方法用于从 JSONArray 除去索引位置中的元素，并返回更新后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.removeValue(object)

此方法用于从 JSONArray 中除去第一次出现的值，并返回更新后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.set(integer index, object value)

此方法用于将 JSONArray 的输入索引设置为输入值，并返回修改后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

此方法用于以整数形式返回 JSONArray 的大小。

对于以下对话运行时上下文：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(String regexp)

此方法用于通过输入正则表达式来拆分输入字符串。结果为字符串的 JSONArray。

对于以下输入：

```
"bananas;apples;pears"
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## 日期和时间
{: #date-time}

有多种方法可用于处理日期和时间。

### .after(String date/time)

- 确定日期/时间值是否晚于日期/时间自变量。
- 类似于 `.before()`。

### .before(String date or time)

- 例如：
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 确定日期/时间值是否早于日期/时间自变量。
- 如果比较不同的项（例如，`时间与日期`、`日期与时间`以及`时间与日期和时间`），那么此方法会返回 false，并且会在响应 JSON 日志 `output.log_messages` 中输出异常。
    - 例如，`@sys-date.before(@sys-time)`。
- 如果比较`日期和时间与时间`，那么此方法会忽略日期而仅比较时间。

### now()

- 静态函数。
- 返回 `yyyy-MM-dd HH:mm:ss` 格式的当前日期和时间的字符串。
- 可以对此函数返回的日期/时间值调用其他日期/时间方法，并且可以将其作为这些方法的自变量传递。
- 如果设置了上下文变量 `$timezone`，那么此函数会返回客户机时区中的日期和时间。

输出字段中使用的具有 `now()` 的对话节点示例：

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

节点条件中的 `now()` 示例（用于确定是否仍是早上）：

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

将日期和时间字符串的格式设置为期望的用户输出格式。

根据指定格式返回已设置格式的字符串：

- `MM/dd/yyyy` 返回 12/31/2016
- `h a` 返回 10pm

要返回星期：

- `E` 表示星期二
- `u` 表示日索引 (1 = 星期一、...、7 = 星期日）

例如，此上下文变量定义会创建 $time 变量，用于将值 17:30:00 保存为 *5:30 PM*。

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

格式遵循 Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) 规则。

>注：尝试仅设置时间的格式时，日期会视为 `1970-01-01`。

### .sameMoment(String date/time)

- 确定日期/时间值是否等于日期/时间自变量。

### .sameOrAfter(String date/time)

- 确定日期/时间值是否晚于或等于日期/时间自变量。
- 类似于 `.after()`。

### .sameOrBefore(String date/time)

- 确定日期/时间值是否早于或等于日期/时间自变量。

有关抽取日期和时间值的系统实体的信息，请参阅 [@sys-date 和 @sys-time 实体](system-entities.html#sys-datetime)。

## 数字
{: #numbers}

### Random()

返回一个随机数。使用以下某个语法选项：

- 要返回随机布尔值（true 或 false），请使用 `<?new Random().nextBoolean()?>`。
- 要返回 0（含 0）和 1（不含 1）之间的随机双精度数，请使用 `<?new Random().nextDouble()?>`
- 要返回 0（含 0）和指定数字之间的随机整数，请使用 `<?new Random().nextInt(n)?>`，其中 n 等于您需要的数字范围上限 + 1。例如，如果要返回 0 到 10 之间的随机数，请指定 `<?new Random().nextInt(11)?>`。
- 要从完整整数值范围（-2147483648 到 2147483648）返回随机整数，请使用 `<?new Random().nextInt()?>`。

例如，可以创建由 #random_number 意向触发的对话节点。第一个响应条件可能类似于：

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

  将对象或字段转换为双精度数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

### toInt()

  将对象或字段转换为整数类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

### toLong()

  将对象或字段转换为长整型数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

有关抽取数字的系统实体的信息，请参阅 [@sys-number 实体](system-entities.html#sys-number)。

## 对象
{: #objects}

### JSONObject.has(string)

如果复杂 JSONObject 具有输入名称的属性，那么此方法会返回 true。

对于以下对话运行时上下文：

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

对话节点输出：

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

结果：条件为 true，因为用户对象包含属性 `first_name`。

### JSONObject.remove(string)

此方法用于从输入 `JSONObject` 中除去名称的属性。此方法返回的 `JSONElement` 是要除去的 `JSONElement`。

对于以下对话运行时上下文：

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

对话节点输出：

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

结果：

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

## 字符串
{: #strings}

### String.append(object)

此方法用于将输入对象作为字符串附加到字符串，并返回修改后的字符串。

对于以下对话运行时上下文：

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

如果字符串包含输入子字符串，那么此方法会返回 true。

输入："Yes, I'd like to go."

使用以下语法：

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

结果：条件为 `true`。

### String.endsWith(string)

如果字符串以输入子字符串结尾，那么此方法会返回 true。

对于以下输入：

```
"What is your name?".
```
{: codeblock}

使用以下语法：

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

结果：条件为 `true`。

### String.extract(String regexp, Integer groupIndex)

此方法用于返回由输入正则表达式的指定组索引所抽取的字符串。

对于以下输入：

```
"Hello 123456".
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **重要信息：**要将 `\\d` 作为正则表达式处理，必须通过再添加一组 `\\` 对原来的两个反斜杠进行转义：`\\\\d`

结果：

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

如果字符串的任何分段与输入正则表达式匹配，那么此方法会返回 true。可以对 JSONArray 或 JSONObject 元素调用此方法，在进行比较之前，此方法会将数组或对象转换为字符串。

对于以下输入：

```
"Hello 123456".
```
{: screen}

使用以下语法：

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

结果：条件为 true，因为输入文本的数字部分与正则表达式 `^[^\d]*[\d]{6}[^\d]*$` 匹配。

### String.isEmpty()

如果字符串为空字符串，而不是空值，那么此方法会返回 true。

对于以下对话运行时上下文：

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

使用以下语法：

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

结果：条件为 `true`。

### String.length()

此方法用于返回字符串的字符长度。

对于以下输入：

```
"Hello"
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

如果字符串与输入正则表达式匹配，那么此方法会返回 true。

对于以下输入：

```
"Hello".
```
{: codeblock}

使用以下语法：

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

结果：条件为 true，因为输入文本与正则表达式 `\^Hello\$` 匹配。

### String.startsWith(string)

如果字符串以输入子字符串开头，那么此方法会返回 true。

对于以下输入：

```
"What is your name?".
```
{: codeblock}

使用以下语法：

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

结果：条件为 `true`。

### String.substring(int beginIndex, int endIndex)

此方法用于获取子字符串，其第一个字符位于 `beginIndex`，最后一个字符设置为索引，位于 `endIndex` 之前。不包括 endIndex 字符。

对于以下对话运行时上下文：

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

此方法用于返回转换为小写字母的原始字符串。

对于以下输入：

```
"This is A DOG!"
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

此方法用于返回转换为大写字母的原始字符串。

对于以下输入：

```
"hi there".
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

此方法用于删除字符串开头和结尾的所有空格，并返回修改后的字符串。

对于以下对话运行时上下文：

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

使用以下语法：

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

生成以下输出：

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

有关用于抽取字符串的系统实体的信息，请参阅[系统实体](system-entities.html)。
